## 1) AutoFillAspect 整个类逐行讲透（按你项目里常见写法）

> 你这个 `AutoFillAspect` 一般出现在 **Mapper 的 insert/update** 前，用 **AOP + 反射** 自动给实体补字段：
> `createTime / updateTime / createUser / updateUser`。

下面我用“典型 sky-take-out 版本”的结构来逐行讲（你实际代码变量名可能略不同，但原理完全一样）。

---

### ✅ 类与注解区

```java
@Aspect
@Component
@Slf4j
public class AutoFillAspect {
```

* `@Aspect`：告诉 Spring **这是一个切面类**（里面有 pointcut + advice）。
* `@Component`：交给 Spring 管理（才能被 AOP 扫描到）。
* `@Slf4j`：生成 `log` 变量（方便打印调试信息）。

---

### ✅ 定义切点（Pointcut）

```java
@Pointcut("execution(* com.sky.mapper.*.*(..)) && @annotation(com.sky.annotation.AutoFill)")
public void autoFillPointCut(){}
```

逐段拆：

* `execution(* com.sky.mapper.*.*(..))`

  * `*` 返回值任意
  * `com.sky.mapper.*` mapper 包下任意类
  * `.*(..)` 任意方法、任意参数
* `&& @annotation(com.sky.annotation.AutoFill)`

  * **只拦截那些方法上“标了 @AutoFill” 的**
  * 这点很关键：否则 mapper 包里所有方法都被切了，会很危险。

`autoFillPointCut()` 这个方法本身通常是空的，它只是一个“切点名字”。

---

### ✅ Advice：在方法执行前填充字段（最常见是 `@Before`）

```java
@Before("autoFillPointCut()")
public void autoFill(JoinPoint joinPoint) {
```

* `@Before`：在目标方法执行 **之前** 做事（先填字段，再入库/更新）。
* `JoinPoint`：能拿到：

  * 目标类/目标方法
  * 方法参数
  * 注解信息（配合反射读取）

---

### ✅ 1) 取到目标方法的注解，判断 INSERT 还是 UPDATE

```java
MethodSignature signature = (MethodSignature) joinPoint.getSignature();
AutoFill autoFill = signature.getMethod().getAnnotation(AutoFill.class);
OperationType operationType = autoFill.value();
```

逐行含义：

* `joinPoint.getSignature()`：拿到“方法签名”（方法名、参数、返回值类型…）
* 强转 `MethodSignature`：因为我们要取到 `Method` 对象
* `getAnnotation(AutoFill.class)`：拿到 `@AutoFill` 注解实例
* `autoFill.value()`：通常是 `INSERT` 或 `UPDATE`

> 这就是：**同一个切面逻辑，根据注解决定填哪些字段**。

---

### ✅ 2) 取到目标方法参数：实体对象（Employee / Dish / Setmeal ...）

```java
Object[] args = joinPoint.getArgs();
if (args == null || args.length == 0) return;
Object entity = args[0];
```

* `getArgs()`：拿到 mapper 方法传进来的参数数组
* 一般 `insert(entity)` / `update(entity)` 的实体就在第一个参数 `args[0]`
* 防御性判断：防止方法没有参数时 NPE

> ⚠️ 陷阱：如果你的 mapper 方法是 `update(@Param("dto") Xxx dto)` 或多个参数，**entity 不一定在 args[0]**（这类要额外处理）。

---

### ✅ 3) 准备要写进去的值：时间 + 当前用户

```java
LocalDateTime now = LocalDateTime.now();
Long currentId = BaseContext.getCurrentId();
```

* `now`：统一时间戳
* `BaseContext.getCurrentId()`：通常来自你 JWT 拦截器解析 token 后放进 ThreadLocal

  * 也就是 “当前登录用户 id”

---

### ✅ 4) 反射调用 set 方法填充字段（核心）

#### INSERT：填 4 个

```java
if (operationType == OperationType.INSERT) {
    entity.getClass().getDeclaredMethod("setCreateTime", LocalDateTime.class).invoke(entity, now);
    entity.getClass().getDeclaredMethod("setCreateUser", Long.class).invoke(entity, currentId);
    entity.getClass().getDeclaredMethod("setUpdateTime", LocalDateTime.class).invoke(entity, now);
    entity.getClass().getDeclaredMethod("setUpdateUser", Long.class).invoke(entity, currentId);
}
```

* `getDeclaredMethod("setXxx", Type.class)`：找到 setter
* `invoke(entity, value)`：在运行时调用这个 setter
* INSERT 要同时写 create + update（因为新建时两者一致）

#### UPDATE：只填 2 个

```java
if (operationType == OperationType.UPDATE) {
    entity.getClass().getDeclaredMethod("setUpdateTime", LocalDateTime.class).invoke(entity, now);
    entity.getClass().getDeclaredMethod("setUpdateUser", Long.class).invoke(entity, currentId);
}
```

---

### ✅ 5) 异常处理（很重要）

```java
} catch (Exception e) {
    log.error("auto fill error", e);
}
```

为什么必须 catch？

* 反射找不到方法：比如实体没有 `setUpdateUser`
* 参数类型不一致：比如你 setter 用的是 `Integer` 但你传 `Long.class`
* `currentId` 为空：JWT 没过拦截器 / ThreadLocal 没设置

> 一句话总结：AutoFillAspect 的本质 = **AOP 定位方法 + 注解决定策略 + 反射给实体补字段**。

---

## 2) AOP 执行顺序 ASCII 图（Spring AOP 调用链）

下面是“一个请求进入后”，AOP/拦截器/Controller 的典型顺序（你项目常见）：

```text
HTTP Request
   |
   v
DispatcherServlet
   |
   v
HandlerInterceptor.preHandle   (比如 JwtTokenAdminInterceptor)
   |    └─ 解析 token -> BaseContext.setCurrentId(id)
   v
Controller method
   |
   v
Service method
   |
   v
Mapper Proxy (MyBatis)
   |
   v
[AOP 进入点]  AutoFillAspect @Before
   |    └─ 读 @AutoFill(INSERT/UPDATE)
   |    └─ 反射 setCreateTime/setUpdateTime/...
   v
Mapper 真正执行 SQL
   |
   v
DB
   |
   v
返回结果 (反向回传)
   |
   v
HandlerInterceptor.afterCompletion
   └─ BaseContext.removeCurrentId()
```

> 关键点：`AutoFillAspect` 发生在 **Mapper 方法真正执行 SQL 之前**，所以它补的字段会直接写进 DB。

---

## 3) 3 道 “AOP 表达式陷阱题” （高频、容易翻车）

### 题 1：`execution(* com.sky.mapper.*.*(..))` 能不能匹配子包？

**问：** 它能匹配 `com.sky.mapper.admin.EmployeeMapper` 吗？
**答：** ❌ 不能。`*` 只匹配一层包。
**正确写法：**

* 匹配所有子包用：`com.sky.mapper..*.*(..)`（`..` 才是递归匹配）

---

### 题 2：`@annotation(AutoFill)` 为什么有时就是不生效？

**问：** mapper 方法明明标了 `@AutoFill`，但 advice 不进。常见原因？
**答：** 高频原因有 3 个：

1. 你切的是 `execution(* com.sky.mapper.*.*(..))`，但实际 mapper 在子包（见题1）
2. 你把注解标在 **接口方法**，但 Spring AOP 拿到的是 **代理实现方法**（某些代理模式下会取不到）
3. 目标方法不是 Spring 管的 Bean（比如你切到了不在容器里的类）

---

### 题 3：`within(...)` 和 `execution(...)` 你乱用会导致“误切太多”

**问：** 如果你写 `within(com.sky.mapper..*)` 会怎样？
**答：** 可能把 mapper 包里 **所有方法都切到**，包括 select 查询；
然后你 advice 里默认拿 `args[0]` 当实体，**查询方法可能没参数或参数不是实体，直接炸**。

**更安全的写法方向：**

* 用 `@annotation(AutoFill)` 精确控制范围
* 或者只切 insert/update 命名：

  * `execution(* com..mapper..*.insert*(..)) || execution(* com..mapper..*.update*(..))`

---

如果你把你真实的 `AutoFillAspect.java` 贴出来（完整类），我可以按你文件**逐行对照你的每一行代码**讲：每行在 JVM 里发生了什么、反射到底调用了哪个 setter、哪些地方最容易 NPE/取不到注解/类型不匹配。
