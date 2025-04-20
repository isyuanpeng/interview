# 袁朋的面试总结

## JAVA基础

### 值传递

Java里面都是值传递，对于基础类型，传递的是值本身，对于引用类型传递的是对象的地址(引用传递：传递的是引用本身，这里的值传递可以理解为引用类型的值就是对象的地址，所以是值传递)

### 基础类型以及所占字节

byte, short, int, long, char, float, double, boolean
1, 2, 4, 8, 2, 4, 8

### 2022-06-21 setter & getter

1. setter 和 gettter 的实际意义

- 为什么要有setter & getter, public dot直接使用不是更方便快捷吗？ 而且setter和getter最终的效果也是读写
    + 安全：程序可以选择只读或只写或读写
    + 灵活：如果不仅仅只是读写属性值，可以自由变换
    + 封装：隐藏内部实现细节
    + 健壮：便于维护，扩展

- 由上一问，为什么public是不安全的呢？
    + 如果写sdk的话，那么有些数据是不想对外修改的，这时候public的作用就体现出来了。如果只是业务代码则，权限体现的不太大。

- 扩展：Java的四大特性 - 直接粘贴

封装

    What：隐藏信息，保护数据访问。
    How：暴露有限接口和属性，需要编程语言提供访问控制的语法。
    Why：提高代码可维护性；降低接口复杂度，提高类的易用性。
    
抽象

    What: 隐藏具体实现，使用者只需关心功能，无需关心实现。
    How: 通过接口类或者抽象类实现，特殊语法机制非必须。
    Why: 提高代码的扩展性、维护性；降低复杂度，减少细节负担。

继承

    What: 表示 is-a 关系，分为单继承和多继承。
    How: 需要编程语言提供特殊语法机制。例如 Java 的 “extends”，C++ 的 “:” 。
    Why: 解决代码复用问题。

多态

    What: 子类替换父类，在运行时调用子类的实现。
    How: 需要编程语言提供特殊的语法机制。比如继承、接口类、duck-typing。
    Why: 提高代码扩展性和复用性。

### 2022-06-22 编译&反编译

编译：源码 -> 字节码
反编译：字节码 -> 源码

#### 什么是字节码，采用字节码的好处是什么？

在 Java 中，JVM 可以理解的代码就叫做字节码（即扩展名为 .class 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以 Java 程序运行时比较高效，而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行。

### String

- 操作少量数据：String
- 单线程操作大量数据：StringBuilder
- 多线程操作大量数据：StringBuffer

### 字符串拼接用“+”还是StringBuilder

Java 语言本身并不支持运算符重载，“+”和“+=”是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。

可以看出，字符串对象通过“+”的字符串拼接方式，实际上是通过 StringBuilder 调用 append() 方法实现的，拼接完成之后调用 toString() 得到一个 String 对象 。

所以使用StringBuilder。

#### 为什么String不可变

##### 什么是不可变

一旦对象被创建并初始化后，内部的状态数据就会保持不变

##### 为什么String不可变

1. 最初的设计就是不可变，设计就是遵循一定的规则，基本约定，体现一致性。
2. final和immutable无关
3. 如果String可变，那么String就可以在方法之前来回穿梭，最终变成什么无人可知。
4. 安全性：String作为hashmap的key值的时候，如果发生改变，则很难被发现
5. 无线程安全问题
6. 所以声明String为final一劳永逸

### HashCode

1. 为什么要有hashCode

如HashSet中检查重复，需要检查对象是否相等.

> 当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashCode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashCode 值作比较，如果没有相符的 hashCode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashCode 值的对象，这时会调用 equals() 方法来检查 hashCode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。

2. HashCode判断相等

- 如果两个对象的hashCode 值相等，那这两个对象不一定相等（哈希碰撞）。
- 如果两个对象的hashCode 值相等并且equals()方法也返回 true，我们才认为这两个对象相等。
- 如果两个对象的hashCode 值不相等，我们就可以直接认为这两个对象不相等。 


### 2022-06-25 为什么不应该通过类实例访问静态成员

如果使用类的实例来访问静态成员的话，那么IDEA会WARNING，那为什么会有这个WARNING呢？

查了一下网上的说明，模糊不清的说法是实例会被回收。

#### 引申问题1 - 静态为什么叫静态？

静态变量 -> 类变量 是继承类的 所有对象共有的
非静态变量 -> 实例变量 具体对象的具体数据 根据对象的变化而变化 是继承对象的

#### 引申问题2 - 什么时候可以定义静态变量或者方法？

所有的类都拥有的共同的属性 -> 静态变量

方法内部没有使用到非静态数据 -> 静态方法

#### 引申知识点

1. 静态的特点
    - 通过类的加载而加载 类一旦被加载进内存，静态的变量和方法就被加载进去了，对象还不存在
    - 静态先于对象存在
    - 被所有的对象共享
    - 可以直接被类名调用，也可以被对象调用

2. 静态使用的注意事项
    - 静态方法只能访问静态成员，不能访问非静态成员
    - 非静态成员可以访问静态成员，也可以访问非静态成员
    - 静态方法中不能出现this,super等关键字

3. 静态的好处和坏处
    - 优点：对于所有对象都共有的数据，节省空间；可以不新建对象直接使用；
    - 缺点：静态是通过类存在的，访问存在局限性，只能访问静态数据


### 为什么https协议调用接口需要SSL认证

1. SSL: Secure Sockets Layer, 安全套接字层，为了解决HTTP协议是明文，避免传输的数据被窃取，篡改，劫持。
2. TSL: Transport Layer Security, 传输层安全协议。TSL是SSL标准化的产物。
3. HTTPS: 兼容HTTP，HTTP over TSL，HTTPS = HTTP + TSL

总之，就是为了安全

#### Java里面怎么做？

1. 如果是Resttemplate的话需要跳过,具体代码如下, 在RestTemplateConfig中加入下面代码:

```
SSLContext sslContext = SSLContexts.custom().loadTrustMaterial(null, (X509Certificate[] chain, String authType) -> true).build();
SSLConnectionSocketFactory csf = new SSLConnectionSocketFactory(sslContext, (s, sslSession) -> true);
CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(csf).setMaxConnPerRoute(1000).setMaxConnTotal(1000).build();
```

2. X509Certificate

X.509是公钥基础设施（PKI）的标准格式。X.509证书就是基于国际电信联盟（ITU）制定的X.509标准的数字证书。

## 集合

### Comparable 和 Comparator 的区别

两个都是用于比较的接口，

- Comparable：出自java.lang, compareTo(Object obj)
- Comparator：出自java.util, compare(Object obj1, Object obj2)

如果只需要一种排序方法，则使用Comparable，如果需要两种以上的，使用Comparator

### 2022-06-29 HashMap

#### 数据结构

数组(每个Node叫做bucket) + 链表 + 红黑树 

主体是一个数组，数组由一个个链表组成，链表由一个个Node组成，Node存储具体的key value

#### 为什么要使用数组+链表的数据结构

解决hash冲突，首先使用hash(key)得出key的hash值，然后通过 (n-1) & hash 得出index，键值对存在该index所在的数组链表中。

不同的key可能产生相同的hash值

1. 如何解决Hash冲突

遇到hash冲突后，会先判断两个key值是否相同，相同说明是同一个key，则覆盖。如果不相同，会将这个Node插入链表的尾部。

2. 头插法和尾插法

头插法：每次在头部插入，作者认为后面插入的使用的概率会更大，会造成死锁的问题
尾插法：每次在尾部插入，JDK1.8后更新

#### 为什么要把链表转换为红黑树？

1. 查找效率

链表：时间复杂度O(n)
红黑树：时间复杂度O(log(n))

链表较短的时候差距不大，较长的时候可以提升效率

2. 为什么不直接使用红黑树的数据结构

红黑树的空间复杂度是链表的2倍

#### 查询的时候如果hash值相同该如何取值

当调用 map.get(p1) 时：

- 首先找到索引位置（100 & (n-1)）
- 遍历该位置的链表
- 对每个节点，先比较哈希值（都是100）
- 然后使用equals()方法比较key
- 当找到name等于"Alice"的Person对象时，返回对应的value

### 2022-06-30 反射

#### 反射是什么？

反射可以在运行时检查类、接口、方法和变量等信息，无需知道类的名字，方法名等。还可以在运行时实例化新对象，调用方法以及设置和获取变量值。

### 2022-07-04 - 07-05 红黑树

6.29日看了hashmap的原理，那么红黑树到底是什么呢？

#### 二叉查找树 BST(Binary Search Tree)

任何一个节点的左子树上的点，都必须小于当前节点。
任何一个节点的右子树上的点，都必须大于当前节点。
任何一颗子树，也都满足上面两个条件。

#### 2-3-4树

4阶的B树，Balance Tree

所有的叶子节点都拥有相同的深度
叶节点只能是2-节点、3-节点、4-节点
元素使用保持排序顺序，父节点大于左子节点，小于右子节点，如果节点有多个元素，则每个元素必须大于它左边的和它左子树中的元素


## JVM

两栈一PC，堆加方法区，前面线程私有，后面线程共享：虚拟机栈和本地方法栈以及程序计数器，堆和方法区。

- 堆：存放对象实例、数组、该区域随着JVM的启动而建立
- 方法区：存放虚拟机加载的类信息数据，线程共享
- 程序计数器：当前线程执行的Java方法的地址信息
- 虚拟机栈：用于存放线程中Java方法执行过程的信息，Java方法的调用和结束结束对应了一个Stack Frame的入栈和出栈。
- 

### 类加载机制

- 类的生命周期：加载-连接-初始化-使用-卸载

- 双亲委派模型：类加载器用于进行Class文件的加载，为此系统提供了三个默认的类加载器，各类加载器之间的层次关系被称为类加载器的双亲委派模型，在双亲委派模型中，当一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的加载器都是如此，因此所有的类加载请求都应该传送到最顶层的启动类加载器中，只有当父加载器无法完成加载请求的时候，子加载器才会尝试自己去完成加载，通过双亲委派模型，可以避免出现类的重复加载。
    + 启动类加载器：用C++实现的，主要负责加载Java_HOME/lib下、classpath参数所制定路径下的类库。
    + 扩展类加载器：加载JAVA_HOME/lib/ext下、java.ext.dirs所指定路径下的类库。
    + 应用程序加载器：系统类加载器，负责加载用户classpath中所有的类库。


## Spring

### @Controller & @RestController

#### @ResponseBody

该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。使用此注解此次请求将不再走视图处理器，而是直接将此响应结果写入到输入流中，其效果等同于使用response对象输出指定格式的数据。

#### @Controller @Service @Compoent

spring在启动时，有一个非常核心的类ConfigurationClassPostProcessor会对类路径下的所以类进行扫描，将符合条件的bean扫描出来添加到beanDefinitionMap集合中，方便接下来的实例化。

#### @Controller

如果不使用springMVC时，三者使用其实是没有什么差别的，但如果使用了springMVC，@Controller就被赋予了特殊的含义。

spring会遍历上面扫描出来的所有bean，过滤出那些添加了注解@Controller的bean，将Controller中所有添加了注解@RequestMapping的方法解析出来封装成RequestMappingInfo存储到RequestMappingHandlerMapping中的mappingRegistry。后续请求到达时，会从mappingRegistry中查找能够处理该请求的方法。

#### @RestController

@Controller + @ResponseBody

### 依赖的版本不一致

一般都是因为自定义版本导致的，而有些依赖会使用spring中写好的版本，所以一般这种情况就更改spring定制的版本号即可，更改就是重新写个全局版本号，会覆盖掉之前的。

### IOC - Inversion of Control 

> 资源不由使用资源的双方管理，而由不使用资源的第三方管理，这可以带来很多好处。第一，资源集中管理，实现资源的可配置和易管理。第二，降低了使用资源双方的依赖程度，也就是我们说的耦合度。

### AOP 

AOP：面向切面编程。简单说，就是把一些业务逻辑中的相同的代码抽取到一个独立的模块中，让业务逻辑更加清爽。

#### 实际业务

实际业务中有以下几个地方会实际使用：

1. 操作日志记录

针对每个方法，创建注解@BusinessLog，凡是加了该注解的方法，创建切面，去记录日志

```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface BusinessLog {

    String moduleId();

    String actionType();

}


@Aspect
@Component
public class BusinessLogAop {

    private static final AriesJcLogger LOGGER = AriesJcLoggerFactory.getLogger(BusinessLogAop.class);

    /**
     * 注解Advice
     */
    @Pointcut("@annotation(com.hikvision.spdd.aac.businesslog.BusinessLog)")
    public void logPoint() {
    }

    /**
     * 方法Advice
     */
    @Pointcut("execution(* com.hikvision.spdd.aac..*Controller.*(..))")
    public void controllerPoint() {

    }

    /**
     * 操作日志切入点，在JoinPoint后，方便获取方法返回结果
     *
     * @param joinPoint   连接点
     * @param returnValue 连接点方法返回参数
     * @return
     * @throws Throwable 抛出异常
     * @author zhenkaicheng 2021/5/31 10:46
     * @since 1.0.0
     */
    @AfterReturning(value = "logPoint()", returning = "returnValue")
    public void afterReturn(JoinPoint joinPoint, Object returnValue) throws Throwable {
        RequestAttributes reqAttrs = RequestContextHolder.getRequestAttributes();
        assert reqAttrs != null;
        HttpServletRequest request = ((ServletRequestAttributes) reqAttrs).getRequest();
        UserBasicInfoDTO user = null;
        try {
            user = (UserBasicInfoDTO) request.getSession().getAttribute(AacConstant.SESSION_USER);
        } catch (Exception e) {
            LOGGER.errorWithErrorCode(AacCommonErrorCode.ERR_SESSION_USER_EMPTY.getCode(),
                    AacCommonErrorCode.ERR_SESSION_USER_EMPTY.getMsg(), e);
        }

        OperationLogBuilder.Builder build = OperationLogBuilder.build();
        build
                //是否支持国际化，0不支持，1支持
                .actionMultiLang(1)
                //0表示web端，1表示客户端
                .terminalType(0);
        //操作用户
        if (user != null) {
            if (OperationLogHolder.getOperationLog() != null) {
                build
                        .userId(user.getUserIndexCode())
                        .userName(user.getUsername())
                        .userOrgId(user.getDeptIndexCode())
                        .userOrgName(user.getDeptName());
            }
        }


        //请求参数
/*        if (joinPoint.getArgs().length > 0) {
            StringBuilder params = new StringBuilder("[");
            Object[] args = joinPoint.getArgs();
            for (int i = 0; i < args.length - 1; i++) {
                params.append(args[i].toString()).append(',');
            }
            params.append(args[args.length - 1].toString());
            params.append("]");
            build
                    .actionParam(params.toString())
                    .actionDetail(params.toString());
        }*/

        //返回结果类型
        if (returnValue != null) {
            BaseResult result = (BaseResult) returnValue;
            try {
                //日志服务中1为成功，对应BaseResult 成功返回是0
                if (AacConstant.LOG_RESULT_FAIL.equals(Integer.valueOf(result.getCode()))) {
                    build.result(OperationLogConst.ActionResult.SUCCESS);
                } else {
                    build.result(OperationLogConst.ActionResult.FAIL);
                    build.errorCode(result.getCode());
                }
            } catch (Exception e) {
                build.result(OperationLogConst.ActionResult.FAIL);
                build.errorCode(result.getCode());
                LOGGER.errorWithErrorCode(AacCommonErrorCode.ERR_PARSE_RETURN_VALUE_ERROR.getCode(),
                        AacCommonErrorCode.ERR_PARSE_RETURN_VALUE_ERROR.getMsg());
            }
        }
        //模块和操作类型
        Signature signature = joinPoint.getSignature();
        String className = joinPoint.getTarget().getClass().getName();
        if (signature instanceof MethodSignature) {
            MethodSignature methodSignature = (MethodSignature) signature;
            Method method = methodSignature.getMethod();
            BusinessLog businessLog = method.getDeclaredAnnotation(BusinessLog.class);
            if (businessLog != null) {
                build
                        .moduleId(businessLog.moduleId())
                        .actionMessageId(OperationLogConst.actionMessageId(className, ((MethodSignature) signature).getMethod().getName()))
                        .action(businessLog.actionType());

            }
            Class<?> declaringClass = method.getDeclaringClass();
            BusinessModule businessModule = declaringClass.getDeclaredAnnotation(BusinessModule.class);
            if (businessModule != null) {
                build
                        .objectType(businessModule.objectType().getKey())
                        .objectName(businessModule.objectType().getName());
            }
        }
        build.log();
    }
}
```

2. 其它案例

在某次基线架构更新的时候，原先通用返回BaseResult是根据type字段去判断返回是否成功的，更新完后，改成了code去判断。那如果旧的程序跑在新的架构平台上，旧的代码中很多接口调用都是使用BaseResult的type去判断是否成功，这时候type字段返回的为null，用type去判断就会造成空指针异常，针对该问题，需要一个通用的解决方案，由于旧的代码中使用type去判断成功的地方非常多，所以需要通用的解决方案。那这里就是使用了AOP切面编程，在某些注解下去修改返回的结果，填充上type字段，这样后续判断的时候就不会触发空指针异常了。

```
@Aspect
@Component
public class ResponseTypeAop {

    private static final AriesJcLogger LOGGER = AriesJcLoggerFactory.getLogger(ResponseTypeAop.class);

    /**
     * feign client post method pointcut
     */
    @Pointcut("@annotation(javax.ws.rs.POST)")
    public void postPoint() {
    }

    /**
     * feign client get method pointcut
     */
    @Pointcut("@annotation(javax.ws.rs.GET)")
    public void getPoint() {
    }

    /**
     * change response BaseResult type pointcut
     */
    @Pointcut("postPoint() || getPoint()")
    public void changeTypePoint() {
    }

    @AfterReturning(value = "changeTypePoint()", returning = "returnValue")
    public void afterReturn(JoinPoint joinPoint, Object returnValue) throws Throwable {
        if (returnValue instanceof BaseResult) {
            BaseResult resultVO = (BaseResult) returnValue;
            String code = resultVO.getCode();
            if ("0".equals(code)) {
                resultVO.setType(0);
            }
        }
    }
}
```

## Maven依赖原则

1. 最短路径原则：优先选择最小路径的依赖，一个项目Demo依赖了两个jar包，其中A-B-C-X(1.0) ， A-D-X(2.0)。由于X(2.0)路径最短，所以项目使用的是X(2.0)
2. pom文件申明顺序优先：如果A-B-X(1.0) ，A-C-X(2.0) 这样的路径长度一样怎么办呢？这样的情况下，maven会根据pom文件声明的顺序加载，如果先声明了B，后声明了C，那就最后的依赖就会是X(1.0)。
3. 覆盖优先原则：子pom内声明的优先于父pom中的依赖。


## 设计模式

### 单一职责原则 Single Responsibility Principle

There should never be more than one reason for a class to change。应该有且仅有一个原因引起类的变更。

意思也很明显，一个类应该有且只有一个引起它变化的原因。

实际案例: 手机Class
V1：MobileClass, 包含了所有功能，charge(), ringUp(), GPU(), CPU(), RAM(), ROM()
V2: BasicProperty,  GPU(), CPU(), RAM(), ROM(); BasicFunction, charge(), ringUp()
V3: BasicProperty, GPU(), CPU(), RAM(), ROM(); ExtendProperty(), Pixel(); BasicFunction, charge(), ringUp(); ExtendFunction, playGame();

没有最好的设计，只有最适合的设计。

### 里氏替换原则 Liskov Substitution Principle

子类对象能够替换父类对象，而程序逻辑不变。

有两种情况，使用共享方法或者是为了多态。

1. 共享方法：子类继承父类为了方法重用，则子类不应该去改变父类中的共享方法。子类和父类都可以实例化，如果子类复写父类的共享类，则违反了LSP，子类对象不能够替换父类对象，逻辑有可能不一致。
2. 多态：如果继承为了多态，则应该将父类方法设为抽象类或者接口，这样子类重新定义父类的方法，父类不能实例化，替换的时候不会造成逻辑上的不一致。

如何符合LSP：尽量不要从可实例化的父类中继承，而是要使用基于抽象类和接口的继承。

## 2023-04-14 Nginx 配置项

1. tcp_nodelay

> Nagle Algorithm: John Nagle的名字来命名的，John Nagle在1984年首次用这个算法来尝试解决福特汽车公司的网络拥塞问题（RFC 896）。如果每次发送一个1 byte的数据包，实际大小需要40(20ip header bytes + 20 tcp header bytes) + 1 byte = 41bytes，Nagle算法解决了该问题，如果包的大小满足MSS，那么可以立即发送，否则放入缓冲区，等到已经发送的包被确认了之后继续发送。

> DelayedAcknowledgment: 如果需要单独确认每个包的大小的话，那么整个网络当中将充斥着无数的ACK，降低网络性能，DelayedAcknowledgment规定: 不再针对单个包发送ACK确认，而是一次确认两个包，或者在发送相应数据的时候捎带这发送ACK，又或者出发超时时间后再发送ACK。

> 上述的两个解决网络性能问题的方法如果在一起使用，会触发延迟问题，如果TCP client端启用了Nagle Algorithm, Server启用 Delayed ACK，并且发送的数据包比较小，Client端需要等待Server端对上一个packet的Ack才能发送当前Packet，Server延迟发送Ack，那整个通信将会被延迟。

- 数据量小，交互多的情况需禁用Nagle算法和Delay，TCP_NODELAY: on
- 数据量大，交互少需要开启，这种情况典型的应用是文件服务器
- 仅长连接使用

2. tcp_nopush

tcp_nopush就是开启linux中的TCP_CORK(塞子), 类似于在发送数据管道处插一个cork，阻塞所有数据，直到取消cork，全部发送阻塞数据。

tcp_nopush和sendfile一起使用。

3. sendfile

正常网络文件传输过程: 

file -> 硬盘 -> kernel buffer -> user buffer -> kernel socket buffer -> 协议栈

senfile网络文件传输过程:

file -> 硬盘 -> kernel buffer(快速拷贝到kernel socket buffer) -> 协议栈

性能提升

>https://www.cnblogs.com/wajika/p/6573014.html


4. lua_shared_dict access 10m

```
user root;
include ../../../../data/global_section/global.conf;


events {
    include ../../../../data/global_section/events.conf;
}
worker_shutdown_timeout 30s;


http {
    include       mime.types;
        # access log on or off
        include ../customizedConfig/access-log.conf;
        absolute_redirect off;
    sendfile   on;
        max_ranges 1;
    tcp_nopush      on;
    tcp_nodelay     on;
        more_clear_headers Server;
        server_tokens   off;
    lua_shared_dict access 10m;
        client_max_body_size  0;
        client_body_buffer_size 1024k;
        large_client_header_buffers 16 64k;
        client_header_buffer_size 16k;
    proxy_connect_timeout 30;
    proxy_read_timeout 600;
    proxy_send_timeout 600;
    proxy_buffer_size 16k;
    proxy_buffers   16 32k;
    proxy_busy_buffers_size 64k;
    proxy_temp_file_write_size 64k;
        proxy_next_upstream error timeout invalid_header http_502 http_503 http_504 http_429;
        lua_shared_dict gaode_shard_dict 10m;
        init_by_lua_file init_section/initIdentify.lua;
        keepalive_timeout 30s;

   proxy_cache_path ../../../data/cache/image_cache levels=1:2 keys_zone=image_cache:1024m inactive=1d max_size=30g;
   proxy_cache_path ../../../data/cache/static_cache levels=1:2 keys_zone=static_cache:1024m inactive=1d max_size=1g;
   open_file_cache max=102400 inactive=20s;
   open_file_cache_valid 30s;
   open_file_cache_min_uses 2;
   open_file_cache_errors on;

        gzip on;

    gzip_min_length 1k; # 太小的没必要压缩
    gzip_buffers 16 8k;
    gzip_proxied any;
    gzip_http_version 1.1;
    gzip_comp_level 3;
    gzip_types  application/x-font-ttf application/font-woff2 text/plain text/css application/javascript application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript ;
        underscores_in_headers on;
        #add_header X-Frame-Options SAMEORIGIN;

    include ../../../../data/global_section/http.conf;
        include ../../../../data/upstream_section/*.conf;
        include ../../../../data/server_section/*.conf;
        include ../../../../data/ssl_certificate/*.conf;

        #init_by_lua_file "init_section/init.lua";

}
```

## 并发

### 死锁

线程A占用资源1，线程B占用资源2，线程A请求资源2，线程B请求资源1.

#### 如何预防死锁

1. 破坏请求与保持条件：一次性申请所有的资源。
2. 破坏不剥夺条件：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
3. 破坏循环等待条件：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

### 线程池配置

- 核心线程数（corePoolSize）

IO密集型：可以大一些，比如CPU核心数的2倍
CPU密集型：通常设置为CPU核心数+1

- 最大线程数（maxPoolSize）

通常设置为核心线程数的2倍或更多
需要考虑系统资源上限

- 队列容量（queueCapacity）

需要根据任务特性设置
太小可能导致频繁创建临时线程
太大可能导致内存问题

- 保活时间（keepAliveTime）

临时线程的空闲存活时间
通常设置为几十秒到几分钟

#### 为什么要设置为CPU核心数+1

CPU密集型任务主要消耗CPU计算资源
线程数等于CPU核心数时，每个核心都在处理一个线程
额外的一个线程用于在以下情况时提供帮助：

某个线程因为各种原因暂时阻塞时
操作系统进行上下文切换时

利用微小的空隙：即使是CPU密集型任务，也会有微小的空隙

等待内存访问
系统调用
线程调度

防止CPU空闲：当某个线程因为以下原因暂时无法运行时

页面错误（Page Fault）
缓存未命中
分支预测失败

```
@Configuration
@EnableAsync
public class ExecutorConfig {
    
    @Bean
    public Executor serviceExecutor() {
        LogUtils.info("start gisschServiceExecutor");
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        //配置核心线程数 核心线程数（corePoolSize）IO密集型：可以大一些，比如CPU核心数的2倍 CPU密集型：通常设置为CPU核心数+1
        executor.setCorePoolSize(9);
        //配置最大线程数
        executor.setMaxPoolSize(16);
        //配置队列大小
        executor.setQueueCapacity(200);
        //配置线程池中的线程的名称前缀
        executor.setThreadNamePrefix("xxx-service-");
        // rejection-policy：当pool已经达到max size的时候，如何处理新任务
        // CALLER_RUNS：不在新线程中执行任务，而是有调用者所在的线程来执行
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        //执行初始化
        executor.initialize();
        return executor;
    }
}
```

### BlockingQueue

高并发场景下，一般都是线程+BlockingQueue组合的方式来处理message，首先创建一个继承了Runnable的类作为消息的处理类，

```
public class ConcurrentThread implements Runnable {
    @Override
    public void run() {
        
    }
}


private final BlockingQueue<GpsInfo> gpsInfoBlockingQueue = new LinkedBlockingQueue<>(QUEUE_MAX_SIZE);


```

## 数据库

### 脏读、幻读、不可重复读

1. 脏读：某个事务去查询时，查询到了还没有提交的数据A，后面该数据回滚了，所以查询到的为null。
2. 幻读：事务B前后两次读取同一个范围的数据，在事务B两次读取的过程中事务A新增了数据，导致事务B后一次读取到前一次查询没有看到的行。
3. 不可重复读：事务B读取了两次数据资源，在这两次读取的过程中事务A修改了数据，导致事务B在这两次读取出来的数据不一致。

- READ-UNCOMMITED读未提交：最低的隔离级别，允许读取尚未提交的数据变更，可能会造成上述三种问题。
- READ-COMMITED读取已提交：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读和不可重复读依然存在。
- REPEATABLE-READ可重复读：对同一字段多次读取结果都是一致的，除非数据本身被修改，可以阻止脏读和不可重复读，但幻读仍然有发生。
- SERIALIZABLE可串行化：事务依次进行，互不干扰。

### 多表联合查询如何优化？

- 热点数据存放redis
- 能够计算得到的数据放到java里面来做
- 使用临时表
- 使用explain查看查询计划，减少不必要的操作
- 多使用where去筛选数据

### ACID事务的四个特性

1. 原子性：事务是一个不可分割的最小单元，事务中的操作要么发生，要么不发生。
2. 一致性：事务在开始和结束以后，数据库的完整性没有被破坏。
3. 隔离性：多个事务并发访问，事务是隔离的
4. 持久性：事务完成后，不会被回滚

## 中间件 



### RabbitMQ

1. RabbitMQ的模式

- simple：一个生产者，多个消费者，一个queue，每条数据被一个消费者消费。
- topic：一个交换机，多个队列，多个消费者，根据routingkey去模糊路由。
- fanout（订阅和发布模式）：一个交换机，没有路由规则，多个队列，多个消费者。
- direct：生产者，一个交换机(directExchange)，路由规则，多个队列，多个消费者。主要根据定义的路由规则决定消息往哪个队列发送 

2. 多服务消费RabbitMQ

采用fanout交换机，每个消费者创建一个专用的queue。但是没有借助数据库，而是通过访问rabbitMQ的API接口，获取这三个队列的所有消费者的IP放到list中，消费者监听到消息后，判断自己的ip是否是ip集合里面的最小值，如果是则消费，如果否则抛弃消息。一旦最小IP的消费者宕机后，则list种就会只剩下两个IP，后续的消息选定的消费者就会从这两个IP中选择最小IP消费。同理它也有第二种方案的缺点。

3. RabbitMQ如何做到高可用？

- 普通集群模式：多个rabbitmq实例，queue只在一个实例上，每个实例同步queue的元数据，你消费的时候，实际上如果连接到了另外一个实例，那么那个实例会从 queue 所在实例上拉取数据过来。这方案主要是提高吞吐量的，就是说让集群中多个节点来服务某个 queue 的读写操作。
- 镜像集群模式：多个实例，多个queue

### Redis

1. redis缓存更新策略

- 旁路缓存：
    + 写：更新db，删除cache
        + 可以先删除在更新吗？不行，可能会造成数据不一致问题，请求1写数据，先删cache，请求2读取数据，请求1更新db
        + 依然会存在数据不一致：请求1从db读数据，请求2更新db中的数据，请求1将数据写入cache
    + 读：从cache中读取，如果有就拿，没有入库，更新cache

2. 数据类型

- String：
    + 简单：String是Redis中最简单的数据类型之一，非常容易使用和理解。
    + 适合小对象：如果你的对象非常小且只包含一个值，使用String通常是更有效的选择，因为它不会引入Hash的额外开销。
    + 无法直接存储多个字段：String只能存储一个值，如果你需要存储多个字段，就需要在应用中实现字段的序列化和反序列化逻辑。
    + 不适合结构化数据：如果你的对象包含多个相关字段，String可能不是最佳选择，因为它无法提供结构化的数据存储。
- Hash：
    + 结构化存储：Hash允许你将一个对象的多个字段和值组织在一个数据结构中，这些字段和值可以具有不同的数据类型（例如字符串、整数等）。这对于存储复杂的数据对象非常有用，因为你可以轻松地添加、删除或更新字段。  
    + 易于扩展：如果对象的字段需要变化，你可以直接在Hash中添加或删除字段，而不必重新设计整个数据模型。这使得Hash适用于数据模型的演化。  
    + 少占用内存：相对于每个字段都存储为单独的String，使用Hash可以节省内存，因为Hash的开销相对较小。
    + 不适合小对象：如果你的对象非常小，只包含少量字段，使用Hash可能会浪费一些内存和存储空间。
    + 难以进行全文搜索：如果需要在对象的文本字段上执行全文搜索操作，使用Hash可能不是最佳选择，因为Hash中的字段不容易进行全文搜索。

