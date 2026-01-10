## ✅ 1) AutoFillAspect「标准写法」（可直接复制）

> 目标：在 **Mapper 的 insert/update** 执行前，通过 **@AutoFill(OperationType)** 自动填充
> `createTime/createUser/updateTime/updateUser`（反射调用实体 setter）。

```java
package com.sky.aspect;

import com.sky.annotation.AutoFill;
import com.sky.constant.AutoFillConstant;
import com.sky.context.BaseContext;
import com.sky.enumeration.OperationType;
import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;
import java.time.LocalDateTime;

/**
 * 自动填充公共字段（createTime/createUser/updateTime/updateUser）
 */
@Aspect
@Component
@Slf4j
public class AutoFillAspect {

    /**
     * 切点：拦截所有 mapper 包下 + 标了 @AutoFill 的方法
     * 注意：.. 表示包含子包
     */
    @Pointcut("execution(* com.sky.mapper..*.*(..)) && @annotation(com.sky.annotation.AutoFill)")
    public void autoFillPointCut() {}

    /**
     * 前置通知：在 SQL 执行前，把公共字段 set 进去
     */
    @Before("autoFillPointCut()")
    public void autoFill(JoinPoint joinPoint) {

        // 1) 拿到目标方法的注解 @AutoFill(OperationType)
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        AutoFill autoFill = signature.getMethod().getAnnotation(AutoFill.class);
        if (autoFill == null) {
            // 理论上不会发生，因为切点已经限定了 @annotation
            return;
        }
        OperationType operationType = autoFill.value();

        // 2) 拿到方法参数（通常第一个就是实体）
        Object[] args = joinPoint.getArgs();
        if (args == null || args.length == 0 || args[0] == null) return;
        Object entity = args[0];

        // 3) 准备要填充的数据
        LocalDateTime now = LocalDateTime.now();
        Long currentId = BaseContext.getCurrentId(); // ThreadLocal 中的当前用户 id（可能为空）

        // 4) 反射调用 entity 的 setter
        try {
            if (operationType == OperationType.INSERT) {
                // setCreateTime(LocalDateTime)
                invokeSetter(entity, AutoFillConstant.SET_CREATE_TIME, LocalDateTime.class, now);
                // setCreateUser(Long)
                invokeSetter(entity, AutoFillConstant.SET_CREATE_USER, Long.class, currentId);
                // setUpdateTime(LocalDateTime)
                invokeSetter(entity, AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class, now);
                // setUpdateUser(Long)
                invokeSetter(entity, AutoFillConstant.SET_UPDATE_USER, Long.class, currentId);
            } else if (operationType == OperationType.UPDATE) {
                invokeSetter(entity, AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class, now);
                invokeSetter(entity, AutoFillConstant.SET_UPDATE_USER, Long.class, currentId);
            }
        } catch (Exception e) {
            log.error("AutoFillAspect autoFill error, entity={}, op={}", entity.getClass().getName(), operationType, e);
        }
    }

    /**
     * 把反射调用抽成一个小工具方法：更干净 + 方便统一异常
     */
    private void invokeSetter(Object target, String methodName, Class<?> paramType, Object value) throws Exception {
        Method method = target.getClass().getDeclaredMethod(methodName, paramType);
        method.setAccessible(true);
        method.invoke(target, value);
    }
}
```

### 配套常量（复制即可）

```java
package com.sky.constant;

public class AutoFillConstant {
    public static final String SET_CREATE_TIME = "setCreateTime";
    public static final String SET_CREATE_USER = "setCreateUser";
    public static final String SET_UPDATE_TIME = "setUpdateTime";
    public static final String SET_UPDATE_USER = "setUpdateUser";
}
```

---

## 🧠 2) AOP + Mapper + Entity 执行链 ASCII 图（一次 INSERT/UPDATE 的全链路）

```text
HTTP Request
   |
   v
DispatcherServlet
   |
   v
JWT Interceptor preHandle
   |  └─ parse token -> BaseContext.setCurrentId(userId)
   v
Controller
   |
   v
Service
   |
   v
MyBatis Mapper Proxy (接口代理对象)
   |
   v
Spring AOP Proxy  (切点命中：@annotation(AutoFill))
   |
   v
@Before AutoFillAspect.autoFill()
   |  ├─ 读取目标方法上的 @AutoFill(OperationType)
   |  ├─ joinPoint.getArgs()[0] 拿到 Entity
   |  └─ 反射调用 Entity.setCreateTime/setUpdateUser...
   v
MyBatis 执行 SQL (insert/update)
   |
   v
DB
   |
   v
Response 返回
   |
   v
JWT Interceptor afterCompletion
   └─ BaseContext.removeCurrentId()
```

**一句话抓重点：**
AOP 在 **Mapper 真正执行 SQL 前** 把字段补到 entity 里，所以 SQL 用到的就是“已经补齐”的对象。

---

## 🧪 3) 高频面试题（AOP + 注解 + 反射）

### 题 1：`@annotation(AutoFill)` 有时拿不到注解，为什么？怎么修？

**答题要点：**

* 可能注解在 **接口方法**，但代理拿到的是 **实现/桥接方法**（某些场景会取不到）
* 解决思路：

  * 用 `MethodSignature` 拿到方法后，再用 `joinPoint.getTarget().getClass()` 去找真正实现方法
  * 或确保注解标在 **最终会被代理调用的方法** 上（常见做法：直接标 mapper 接口方法 + 使用 Spring/MyBatis 常规代理通常 OK，但要懂“为什么有时不行”）

---

### 题 2：为什么切点写成 `com.sky.mapper.*.*(..)` 会“突然失效”？

**答题要点：**

* `*` 只匹配一层包，不匹配子包
  `com.sky.mapper.admin.EmployeeMapper` 不会命中
* 标准修法：用 `..` 递归匹配子包
  `execution(* com.sky.mapper..*.*(..))`

---

### 题 3：反射填充字段时，最容易踩哪些坑？你怎么让它更健壮？

**答题要点：**

* 坑点：

  1. `args[0]` 不一定是实体（方法多个参数 / DTO 包装 / @Param）
  2. setter 参数类型不一致（Long vs Integer）
  3. `currentId` 为空（未登录、token 没过拦截器、异步线程 ThreadLocal 丢失）
  4. 实体没有某个 setter（编译没问题，运行反射直接炸）
* 提升健壮性：

  * 对参数做类型判断（例如只对包含这些 setter 的对象执行）
  * 反射找不到方法时“降级跳过”而不是全局报错
  * 异步场景传递 userId（不要依赖 ThreadLocal 自动继承）

---

如果你愿意，我还能给你一版“更强壮的工业版”：

* 支持 **多参数 mapper**（从 args 里自动定位实体对象）
* 支持 **缓存 Method**（减少反射开销）
* 对 **currentId 为空** 做策略（比如更新允许 null / 或直接抛业务异常）
