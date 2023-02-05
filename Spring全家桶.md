```git
# 解决文件过大问题 删除
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch collisionavoidance/librealsense/librealsense2.so" --prune-empty  --tag-name-filter cat -- --all
```



## 1.使用Maven构建项目

1.1`Archetype`选择最后的`org.apache.maven.archetypes:maven-archetype-webapp`

1.2 添加`Spring5`的依赖

```maven
<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.23</version>
</dependency>
```

1.3 在`src\main\`下新建`java`文件夹，这个地方可以写Java文件

1.4 在`resources`文件下可以新建`xml文件`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--配置 User 对象创建-->
    <bean id="user" class="beans.User"></bean>
</beans>

（1）在 spring 配置文件中，使用 bean 标签，标签里面添加对应属性，就可以实现对象创建
（2）在 bean 标签有很多属性，介绍常用的属性
* id 属性：唯一标识
* class 属性：类全路径（包类路径）
name 也是有的
（3）创建对象时候，默认也是执行无参数构造方法完成对象创建
```

1.5 在`webapp`文件下可以新建前端页面，例如`jsp`

## 2.IOC(概念与原理)

1、什么是 IOC

`（1）控制反转，把对象创建和对象之间的调用过程，交给 Spring 进行管理`

`（2）使用 IOC 目的：为了耦合度降低`

`（3）做入门案例就是 IOC 实现`

2、IOC 底层原理

`（1）xml 解析、工厂模式、反射`

3、画图讲解 IOC 底层原理

两部，第一步是xml配置文件，配置创建的对象，第二步是通过反射去创建对象（这个地方是工厂类）

**IOC（BeanFactory 接口）**

1、IOC 思想基于 IOC 容器完成，**IOC 容器底层就是对象工厂**

2、Spring 提供 IOC 容器实现两种方式：（两个接口）

（1）BeanFactory：IOC 容器基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用

* 加载配置文件时候不会创建对象，在**获取对象（使用）才去创建对象**

（2）ApplicationContext：BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人员进行使

* 加载配置文件时候就会把在配置文件对象进行创建3、ApplicationContext 接口有实现类

3、ApplicationContext 接口有实现类

**IOC 操作 Bean 管理（概念）**

1、什么是 Bean 管理（0）Bean 管理指的是两个操作（1）Spring 创建对象（2）Spirng 注入属性

2、Bean 管理操作有两种方式（1）基于 xml 配置文件方式实现（2）基于注解方式实现

`DI：依赖注入，就是注入属性`

```xml
1）第一种注入方式：使用 set 方法进行注入
2）第二种注入方式：使用有参数构造进行注入
3）p 名称空间注入（了解）
<beans ---
xmlns:p="http://www.springframework.org/schema/p"
----
<bean id="book" class="com.atguigu.spring5.Book" p:bname="九阳神功"
p:bauthor="无名氏"></bean>

1、字面量
（1）null 值
<!--null 值-->
<property name="address">
 <null/>
</property>
（2）属性值包含特殊符号
<!--属性值包含特殊符号
 1 把<>进行转义 < >
 2 把带特殊符号内容写到 CDATA
-->
<property name="address">
 <value><![CDATA[<<南京>>]]></value>
</property>
```

3、注入属性-外部 bean

（1）创建两个类 service 类和 dao 类（2）在 service 调用 dao 里面的方法（3）在 spring 配置文件中进行配置

4、注入属性-内部 bean

（1）一对多关系：部门和员工一个部门有多个员工，一个员工属于一个部门。部门是一，员工是多

（2）在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示

5、注入属性-级联赋值

6.`bean 生命周期有七步`（1）通过构造器创建 bean 实例（无参数构造）（2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）（3）把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization（4）调用 bean 的初始化的方法（需要进行配置初始化的方法）（5）把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization（6）bean 可以使用了（对象获取到了）（7）当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）

**IOC 操作 Bean 管理(基于注解方式)**

1、什么是注解（1）注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值..)（2）使用注解，注解作用在类上面，方法上面，属性上面（3）使用注解目的：简化 xml 配置

2、Spring 针对 Bean 管理中创建对象提供注解

（1）@Component（2）@Service（3）@Controller（4）@Repository

* 上面四个注解功能是一样的，都可以用来创建 bean 实例

3、基于注解方式实现对象创建

第一步 引入依赖

第二步 开启组件扫描

第三步 创建类，在类上面添加创建对象注解

`开启组件扫描细节配置`

```xml
<!--开启组件扫描
 1 如果扫描多个包，多个包使用逗号隔开
 2 扫描包上层目录
-->
<context:component-scan base-package="com.atguigu"></context:component-scan>

<!--示例 1
 use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
 context:include-filter ，设置扫描哪些内容
-->
    <context:component-scan base-package="com.codeking" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
    </context:component-scan>
    <!--示例 2
     下面配置扫描包所有内容
     context:exclude-filter： 设置哪些内容不进行扫描
    -->
    <context:component-scan base-package="com.codeking" use-default-filters="false">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
```

`基于注解方式实现属性注入`

（1）@Autowired：根据属性类型进行自动装配第一步 把 service 和 dao 对象创建，在 service 和 dao 类添加创建对象注解第二步 在 service 注入 dao 对象，在 service 类添加 dao 类型属性，在属性上面使用注解

（2）@Qualifier：根据名称进行注入这个@Qualifier 注解的使用，和上面@Autowired 一起使用

（3）@Resource：可以根据类型注入，可以根据名称注入

（4）@Value：注入普通类型属性 

```java
@Value(value = "abc")
private String name;
```

## 3.AOP

1、什么是 AOP

```
（1）面向切面编程（方面），利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，
    提高程序的可重用性，同时提高了开发的效率。
（2）通俗描述：不通过修改源代码方式，在主干功能里面添加新功能
（3）使用登录例子说明 AOP
```

2.AOP（底层原理）

1、AOP 底层使用动态代理

（1）有两种情况动态代理

第一种 有接口情况，使用 JDK 动态代理

⚫ 创建接口实现类代理对象，增强类的方法

第二种 没有接口情况，使用 CGLIB 动态代理

⚫ 创建子类的代理对象，增强类的方法

2.AOP 操作（准备工作）

```java
1、Spring 框架一般都是基于 AspectJ 实现 AOP 操作
（1）AspectJ 不是 Spring 组成部分，独立 AOP 框架，一般把 AspectJ 和 Spirng 框架一起使
用，进行 AOP 操作
2、基于 AspectJ 实现 AOP 操作
（1）基于 xml 配置文件实现
（2）基于注解方式实现（使用）
3、在项目工程里面引入 AOP 相关依赖(直接用maven)
4、切入点表达式
（1）切入点表达式作用：知道对哪个类里面的哪个方法进行增强
（2）语法结构： execution([权限修饰符] [返回类型] [类全路径] [方法名称]([参数列表]) )
举例 1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强 ..代表参数列表
execution(* com.atguigu.dao.BookDao.add(..))
举例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强
execution(* com.atguigu.dao.BookDao.* (..))
举例 3：对 com.atguigu.dao 包里面所有类，类里面所有方法进行增强
execution(* com.atguigu.dao.*.* (..))
1、创建类，在类里面定义方法
public class User {
 public void add() {
 System.out.println("add.......");
 }
}
2、创建增强类（编写增强逻辑）
（1）在增强类里面，创建方法，让不同方法代表不同通知类型
//增强的类
public class UserProxy {
 public void before() {//前置通知
 System.out.println("before......");
 }
}

```

## 4.事务管理

`事务操作（Spring 事务管理介绍）`

```java
1、事务添加到 JavaEE 三层结构里面 Service 层（业务逻辑层）
2、在 Spring 进行事务管理操作（1）有两种方式：编程式事务管理和 声明式事务管理（使用）
3、声明式事务管理（1）基于注解方式（使用）（2）基于 xml 配置文件方式
4、在 Spring 进行声明式事务管理，底层使用 AOP 原理
5、Spring 事务管理 API（1）提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类
```
