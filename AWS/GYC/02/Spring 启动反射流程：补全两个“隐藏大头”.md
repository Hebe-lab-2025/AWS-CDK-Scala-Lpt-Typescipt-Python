你这份笔记整体是对的，我帮你做三件事：**补严谨点 + 把关键坑补上 + 给你一份“最稳 AutoFillAspect 反射修复模板”**（可直接用）。

---

# 1) Spring 启动反射流程：补全两个“隐藏大头”

你图里少了两个常被问的点：

### A. 扫描阶段并不“加载类执行代码”

Spring 扫描通常是读 **class metadata**（ASM/metadata reader），不会立刻触发静态代码块；真正创建对象才会反射调用构造器。

### B. 反射注入不只 Field.set

Spring 更偏好走 `BeanWrapper`（底层还是反射），**setter 注入 / 字段注入 / 构造器注入**都可能。

你这张 ASCII 我建议加两行（面试更稳）：

```text
[Scan classpath -> read metadata (ASM)]
        |
        v
[Create BeanDefinition (no instance yet)]
        |
        v
[Create instance] -> reflect constructor
        |
        v
[Dependency injection] -> BeanWrapper/Reflection
```

---

# 2) @Autowired 注入：你写的对，但要加“注入规则顺序”更精确版

你写的是：type -> qualifier/primary -> name
更精确的“背诵版”是：

* **按类型找候选**（byType）
* 候选 >1：

  * 有 `@Qualifier`：按 qualifier 精确匹配
  * 否则有 `@Primary`：选 primary
  * 否则按**字段名/参数名**匹配 beanName
  * 还不行：报 `NoUniqueBeanDefinitionException`

还有一个很常考的点：
**构造器注入**如果只有一个构造器，Spring 会默认用它（即使没写 `@Autowired`，新版本是这样）。

---

# 3) AOP 代理：补一句“什么时候你拿到的是 proxy？”

你写的“after-init 创建 proxy”很关键，再补一句面试很加分：

* **注入发生在创建 bean 的过程中**，但 Spring 会尽量保证：

  * **其他 bean 注入到的是最终暴露对象**（可能是代理）
* 所以 Controller 注入 Service 时，拿到的通常是 **代理后的 Service**（如果命中切面）。

---

# 4) 你提到的坑：`NoSuchMethodException setUpdateTime(...)` —— 典型根因清单

你已经点到“setter 找不到”，我再把**最常见 4 个根因**列成排查清单：

1. **方法名不一致**

   * `setUpdateTime` vs `setUpdate_Time` / `setUpdate_time` / Lombok 生成名不一样（字段名奇怪时会翻车）
2. **参数类型不一致**

   * 你反射找的是 `LocalDateTime`，但实体是 `Date` / `Timestamp`
3. **方法不可见 or 在父类/接口上**

   * `getMethod` 只能拿 public（含继承），`getDeclaredMethod` 才拿本类私有（但不含继承）
4. **传入对象不是你想的实体类**

   * 例如你切面拿到的是 DTO、Map、或代理对象字段在父类

---

# 5) ✅ 最稳的 AutoFillAspect 反射修复模板（带缓存 + 类型兜底 + Field 兜底）

下面这份思路是：
**优先 setter（更标准）→ 不行就直接改字段（兜底）→ 还不行才报错**
并且做了 **Method 缓存**（避免每次反射查找）。

```java
import java.lang.reflect.*;
import java.time.LocalDateTime;
import java.util.*;
import java.util.concurrent.ConcurrentHashMap;

public class ReflectionFillUtil {

    // 缓存：Class -> (methodName -> Method)
    private static final Map<Class<?>, Map<String, Method>> METHOD_CACHE = new ConcurrentHashMap<>();

    // ========= 对外入口：填充 updateTime / updateUser 示例 =========
    public static void fillUpdateFields(Object entity, Long userId) {
        if (entity == null) return;

        LocalDateTime now = LocalDateTime.now();

        // 1) updateTime
        setValue(entity, "updateTime", now);

        // 2) updateUser
        setValue(entity, "updateUser", userId);
    }

    // ========= 核心：尽量用 setter，失败再 Field 兜底 =========
    private static void setValue(Object target, String fieldName, Object value) {
        Class<?> clazz = target.getClass();

        // 1) 尝试 setter：setXxx(...)
        String setterName = "set" + capitalize(fieldName);

        // 1.1 先按 value 的精确类型找
        Method m = findSetter(clazz, setterName, value == null ? null : value.getClass());

        // 1.2 如果找不到，做“常见类型兜底”（比如 Long vs long，Date vs LocalDateTime 等）
        if (m == null) {
            m = findCompatibleSetter(clazz, setterName, value);
        }

        if (m != null) {
            try {
                m.setAccessible(true);
                m.invoke(target, convertIfNeeded(m.getParameterTypes()[0], value));
                return;
            } catch (Exception ignore) {
                // 继续走字段兜底
            }
        }

        // 2) 字段兜底：直接写 field
        Field f = findField(clazz, fieldName);
        if (f != null) {
            try {
                f.setAccessible(true);
                f.set(target, convertIfNeeded(f.getType(), value));
                return;
            } catch (Exception e) {
                throw new RuntimeException("Failed to set field '" + fieldName + "' on " + clazz.getName(), e);
            }
        }

        // 3) 最后才抛错：说明实体不支持这个字段
        throw new RuntimeException("No setter/field found for '" + fieldName + "' on " + clazz.getName());
    }

    // ========= Method 查找（带缓存） =========
    private static Method findSetter(Class<?> clazz, String setterName, Class<?> paramType) {
        if (paramType == null) return null;

        Map<String, Method> classCache = METHOD_CACHE.computeIfAbsent(clazz, k -> new ConcurrentHashMap<>());
        String key = setterName + "#" + paramType.getName();

        if (classCache.containsKey(key)) {
            return classCache.get(key);
        }

        // getMethods()：包含父类 public 方法
        for (Method method : clazz.getMethods()) {
            if (method.getName().equals(setterName) && method.getParameterCount() == 1) {
                Class<?> p = method.getParameterTypes()[0];
                if (isAssignable(p, paramType)) {
                    classCache.put(key, method);
                    return method;
                }
            }
        }

        classCache.put(key, null);
        return null;
    }

    // 兼容查找：比如 Long -> long，LocalDateTime -> Date 等（按你项目需要加）
    private static Method findCompatibleSetter(Class<?> clazz, String setterName, Object value) {
        for (Method method : clazz.getMethods()) {
            if (!method.getName().equals(setterName) || method.getParameterCount() != 1) continue;

            Class<?> p = method.getParameterTypes()[0];

            // value 为空：只要有这个 setter 就行
            if (value == null) return method;

            // 直接可赋值
            if (isAssignable(p, value.getClass())) return method;

            // 常见：Long -> long
            if ((p == long.class && value.getClass() == Long.class) ||
                (p == Long.class && value.getClass() == long.class)) return method;

            // 你可以按项目需要扩展：LocalDateTime -> Date/Timestamp 等
        }
        return null;
    }

    private static boolean isAssignable(Class<?> targetType, Class<?> valueType) {
        // 处理 primitive / wrapper 的基本兼容
        if (targetType.isPrimitive()) {
            return primitiveToWrapper(targetType).isAssignableFrom(valueType);
        }
        return targetType.isAssignableFrom(valueType);
    }

    private static Class<?> primitiveToWrapper(Class<?> primitive) {
        if (primitive == int.class) return Integer.class;
        if (primitive == long.class) return Long.class;
        if (primitive == boolean.class) return Boolean.class;
        if (primitive == double.class) return Double.class;
        if (primitive == float.class) return Float.class;
        if (primitive == char.class) return Character.class;
        if (primitive == byte.class) return Byte.class;
        if (primitive == short.class) return Short.class;
        return primitive;
    }

    // 类型转换兜底（按项目扩展）
    private static Object convertIfNeeded(Class<?> targetType, Object value) {
        if (value == null) return null;

        // primitive 目标类型，允许 wrapper 直接赋值（反射会自动拆箱）
        if (targetType.isPrimitive()) return value;

        // 直接兼容
        if (targetType.isAssignableFrom(value.getClass())) return value;

        // 这里你可以扩展：LocalDateTime -> Date / Timestamp / String 等
        return value;
    }

    private static Field findField(Class<?> clazz, String name) {
        Class<?> cur = clazz;
        while (cur != null && cur != Object.class) {
            try {
                return cur.getDeclaredField(name);
            } catch (NoSuchFieldException ignore) {
                cur = cur.getSuperclass();
            }
        }
        return null;
    }

    private static String capitalize(String s) {
        if (s == null || s.isEmpty()) return s;
        return Character.toUpperCase(s.charAt(0)) + s.substring(1);
    }
}
```

### 这份模板为什么“稳”

* **不依赖实体一定有 setter**（Field 兜底）
* **兼容 primitive/wrapper**
* **Method 缓存**（切面频繁执行不抖）
* 错误信息也更清晰（直接告诉你哪个字段缺失）

---

# 6) 你要的“AutoFillAspect 调用链 ASCII”（超常考）

```text
Controller -> Service(proxy) -> (AOP)
                 |
                 v
          @Around / @Before
                 |
                 v
     获取 args[0] = entity
                 |
                 v
  ReflectionFillUtil.fillUpdateFields(entity, userId)
                 |
                 v
  先找 setUpdateTime(...) -> invoke
       找不到 -> 直接写 updateTime 字段
                 |
                 v
           proceed() 调真实方法
```

---

如果你把你项目里 **实体类（字段名）** + 你切面里反射调用那几行贴出来，我可以把上面的 `convertIfNeeded()` 和 “兼容 setter 查找” **精确对齐到你现在的 LocalDateTime/Long/Date/Timestamp 的真实类型**，让 `NoSuchMethodException` 彻底消失。
