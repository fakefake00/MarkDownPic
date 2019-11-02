# 1、Spring

---

[Spring](https://repo.spring.io/release/org/springframework/spring/)
<https://github.com/spring-projects/spring-framework/>

---

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>5.2.0.RELEASE</version>
</dependency>
```


- Spring是一个开源的免费的框架
- Spring是一个轻量级、非入侵的框架
- 控制反转（IOC）、面向切面编程（AOP）
- 支持事务的处理、对框架整合的支持

 

# 2、IOC本质

**控制反转IoC(*Inversion of Control*)，是一种设计思想，DI(_依赖注入_)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

采用**XML**方式配置==Bean==的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）**。



# 3、HelloSpring

# 4、IOC创建对象的方式

* 下标赋值

    ```xml
    <!--第一种，下标赋值-->
    <bean id="user" class="cn.hui.domain.User">
        <constructor-arg index="0" value="滴滴"/>
    </bean>
    ```

    

* 类型

    ```xml
    <!--第二种，通过类型创建，（如果构造器有两个String类型参数）不建议使用-->
    <bean id="user" class="cn.hui.domain.User">
        <constructor-arg type="java.lang.String" value="敌众敌"/>
    </bean>
    ```

    

* 参数名

    ```xml
    <!--第三种，直接通过参数名来设置-->
    <bean id="user" class="cn.hui.domain.User">
        <constructor-arg name="name" value="弟弟"/>
    </bean>
    ```

    

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了。  
# 5、Spring配置



## 5.1、别名

```xml
<!--别名，如果添加了别名，可以使用别名获取到这个对象-->
    <alias name="user" alias="userNew"/>
```

## 5.2、Bean的配置

```xml
<!--
    id：bean的唯一标识符，也就是相当于我们学的对象名
    class：bean对象所对应的全限定名：包名+类型
    name：也是别名，而且name更强大，可以同时取多个别名
-->
<bean id="user2" class="cn.hui.domain.User2" name="u22 u2,u3;u4">
    <property name="name" value="说服力"/>
</bean>
```

## 5.3、import

# 6、依赖注入

## 6.1、构造器注入

## 6.2、Set方式注入【重点】

- 依赖注入：Set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，有容器来注入

【环境搭建】

1. 复杂类型

   ```java
   public class Address {
       private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   }
   ```

   

2. 真实测试对象

   ```java
   public class Student {
       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbies;
       private Map<String,String> card;
       private Set<String> games;
       private String wife;
       private Properties info;
   }
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   	<!-- 第一种，普通值注入，value -->
       <bean id="student" class="cn.hui.pojo.Student">
           <property name="name" value="胖子"/>
       </bean>
   
   </beans>
   ```

4. 测试类

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student stu = (Student) context.getBean("student");
   
           System.out.println(stu.getName());
       }
   }
   
   ```

5. 完善注入信息

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="address" class="cn.hui.pojo.Address">
           <property name="address" value="莆田"/>
       </bean>
   
       <bean id="student" class="cn.hui.pojo.Student">
   
           <!--第一种，普通值注入，value-->
           <property name="name" value="胖子"/>
           <!--第二种，Bean注入，ref-->
           <property name="address" ref="address" />
           <!--数组-->
           <property name="books">
               <array>
                   <value>红楼梦</value>
                   <value>水浒传</value>
                   <value>西游记</value>
                   <value>三国演义</value>
               </array>
           </property>
           <!--List-->
           <property name="hobbies">
               <list>
                   <value>听歌</value>
                   <value>敲代码</value>
                   <value>看电影</value>
               </list>
           </property>
           <!--Map-->
           <property name="card">
               <map>
                   <entry key="身份证" value="111111111111"></entry>
                   <entry key="银行卡" value="222222222222"></entry>
               </map>
           </property>
           <!--Set-->
           <property name="games">
               <set>
                   <value>BOB</value>
                   <value>COC</value>
                   <value>LOL</value>
               </set>
           </property>
           <!--null-->
           <property name="wife">
               <null/>
           </property>
           <!--Properties-->
           <property name="info">
               <props>
                   <prop key="学号">2188102108</prop>
                   <prop key="姓名">小明</prop>
                   <prop key="性别">男</prop>
               </props>
           </property>
   
       </bean>
   </beans>
   ```

   

## 6.3、拓展方式注入

可以使用p命名空间和c命名空间进行注入

官方解释：

![image-20191025001927715](https://github.com/fakefake00/repo1/raw/master/Spring.assets/image-20191025001927715.png)

使用：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入，可以直接注入属性的值：property-->
    <bean id="user" class="cn.hui.pojo.User" p:age="19" p:name="阿威"></bean>

    <!--c命名空间注入，通过构造器注入：constructor-args -->
    <bean id="user2" class="cn.hui.pojo.User" c:age="23" c:name="阿友"></bean>
    
</beans>
```

测试：

```java
@Test
public void test1(){
    ApplicationContext context = new ClassPathXmlApplicationContext("beansUser.xml");
    User user = context.getBean("user2", User.class);
    System.out.println(user.toString());
}
```

注意点：p命名和c命名空间不能直接使用，需要导入xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

## 6.4、bean的作用域

![image-20191025090622684](https://github.com/fakefake00/repo1/raw/master/Spring.assets/image-20191025090622684.png)

1. 单例模式（Spring默认机制）

   ```xml
   <bean id="user2" class="cn.hui.pojo.User" c:age="23" c:name="阿友" scope="singleton"></bean>
   ```

2. 

   ```xml
   <bean id="user2" class="cn.hui.pojo.User" c:age="23" c:name="阿友" scope="prortotype"></bean>
   ```

3. 其余的request、session、application、这些个只能在web开发中使用到！  

# 7、Bean的自动装配

- 自动装配是Spring满足bean依赖一种方式
- Spring会在上下文中自动寻找，并自动给bean装配属性



在Spring中有三种装配方式：

1. 在xml中显示的配置
2. 在java中显示配置
3. 隐式的自动装配bean【重点】



### 7.1、测试

- ByName自动装配

  ```xml
  <bean id="cat" class="cn.hui.domain.Cat"/>
  <bean id="dog" class="cn.hui.domain.Dog"/>
  
  <bean id="person" class="cn.hui.domain.Person" autowire="byName">
      <property name="name" value="小明"/>
      <!--        <property name="cat" ref="cat"/>-->
      <!--        <property name="dog" ref="dog"/>-->
  </bean>
  ```

- ByType自动装配

  ```xml
  <bean  class="cn.hui.domain.Cat"/>
  <bean  class="cn.hui.domain.Dog"/>
  
  <bean id="person" class="cn.hui.domain.Person" autowire="byType">
      <property name="name" value="小明"/>
      <!--        <property name="cat" ref="cat"/>-->
      <!--        <property name="dog" ref="dog"/>-->
  </bean>
  ```

  小结：

  - byName，需要保证bean的id唯一，并且需要和自动注入的属性的set方法的值一致。
  - byType，需要保证所有的bean的class唯一，并且需要和自动注入的属性的类型一致。

  ### 

### 7.2、使用注解实现自动装配

jdk1.5支持的注解，Spring2.5就支持注解了

 The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML. 



要使用注解须知：

1. 导入约束：context约束

2. 配置注解的支持：==<context:annotation-config/>==

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
   </beans>
   ```

  @Autowired

- 直接在属性上使用即可，也可以在set方式上使用。
- 使用Autowired可以不用编写Set方法，前提是你这个自动装配的属性在IOC(Spring)容器中存在，且符合名字byName。



科普：

@Nullable 字段标记了这个注解，说明这个字段可以为null；



```java
public @interface Autowired {
    boolean required() default true;
}
```

测试代码：

```java
public class Person {
    //如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解@Autowired完成的时候，可以使用@Qualifier(value="xxx")去配合@Autowired的使用，指定一个唯一的bean对象注入。

```java
public class Person {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog111")
    private Dog dog;
    private String name;
}
```



**@Resource注解**

```java
public class Person {
    @Resource(name="cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

小结：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在（常用）
- @Resource默认通过byName的方式思想，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错（常用）
- 执行顺序不同：Autowired通过byType的方式实现

# 8、使用注解开发

在Spring4之后，要使用注解开发，必须保证aop的包导入了

![image-20191025154442453](https://github.com/fakefake00/repo1/raw/master/Spring.assets/image-20191025154442453.png)

使用注解需要导入context约束，增加注解的支持



1. bean

2. 属性如何注入

   ```java
   //@Component组件等价于<bean id="user" class="com.hui.pojo.User"/>
   @Component
   public class User {
   
       //相当于<property name="name" value="筱筱"/>
       @Value("筱筱")
       public String name;
   }
   ```

   

3. 衍生的注解

   @Component有几个衍生注解，在web开发中，会按照mvc三次架构分层

   - dao【@Repository】

   - service【@Service】

   - controller【@Controller】

     这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配bean

4. 自动装配

   ```
   @Autowired:自动装配通过类型。名字
   @Nullable:字段标记了这个注解，说明这个字段可以为null
   @Resource:自动装配通过名字。类型。
   ```

5. 作用域

   ```java
   @Component
   @Scope("prototype")
   public class User {
   
       //相当于<property name="name" value="筱筱"/>
       @Value("筱筱")
       public String name;
   }
   ```

6. 小结 

   xml 与 注解：

   - xml更加万能，适用于任何场合，维护简单方便
   - 注解 不是自己类使用不了，维护相对复杂

   xml 与 注解最佳实践：

   - xml用来管理bean；

   - 注解只负责完成属性的注入

   - 在使用时注意一个问题：必须让注解生效，就需要开启注解的支持

     ```xml
     <!--指定要扫描的包，这个包下的注解就会生效，不在这个包下的注解就无效了-->
     <context:component-scan base-package="com.hui.pojo"/>
     <context:annotation-config/>
     ```



# 9、使用Java的方式配置Spring

现在要完全不使用Spring的xml配置了，全权交给Java来做。

JavaConfig时Spring的一个子项目，在Spring4之后变成核心功能



实体类：

```java
@Component  //这个注解说明这个类被Spring接管了，注册到了容器中
public class User {

    private String name;

    public String getName() {
        return name;
    }

    @Value("大哥")    //属性注入值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置文件：

```java
//这个也会Spring容器托管，注册到容器中，因为它本身底层就是一个@Component
//@Configuration代表这是一个配置类，就和之前看的beans.xml
@Configuration
@ComponentScan("com.hui.pojo")
@Import(WorldConfig.class)
public class HelloConfig {

    //注册一个bean，就相当于之前写的一个bean标签
    //这个方法的名字，相当于baan标签中的id属性
    //方法的返回值，相当于bean标签中的class属性
    @Bean
    public User getUser(){
        return new User();  //就是返回要注入到bean的对象
    }
}
```

测试类：

```java
public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类方式去做，就只能通过AnnotationConofig上下文来获取容器，通过配置类的class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(HelloConfig.class);
        User user = context.getBean("getUser", User.class);
        System.out.println(user.getName());
    }
}
```

这种纯Java的配置方式，在SpringBoot中随处可见



# 10、代理模式

为什么学代理模式？它是SpringAOPD的底层。

### 10.1、静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，一般会做一些附属操作
- 客户：访问代理 对象的人



代理模式好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务；
- 公共业务也就交给代理角色，实现业务的分工；
- 公共业务发生扩张的时候，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率变低。



案例代码步骤：

1. 接口

   ```java
   public interface Rent {
       public void rent();
   }
   ```

2. 真实角色

   ```java
   public class Host implements Rent {
       public void rent() {
           System.out.println("房东要出租房子了");
       }
   }
   ```

3. 代理角色

   ```java
   public class Proxy implements Rent{
       private Host host;
   
       public Proxy() {
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       public void rent() {
           host.rent();
           seeHouse();
           heTong();
           Fare();
       }
   
       //看房
       public void seeHouse(){
           System.out.println("中介带你看房");
       }
       //合同
       public void heTong(){
           System.out.println("签租赁合同");
       }
       //收中介费
       public void Fare(){
           System.out.println("收中介费");
       }
   }
   ```

   

4. 客户端访问代理角色

   ```java
   public class Client {
       public static void main(String[] args) {
           //房东要出租房子
           Host host = new Host();
           //代理，中介帮房东出租房子。但代理角色会有一些附属操作
           Proxy proxy = new Proxy(host);
           //你不要面对房东，直接找中介租房即可
           proxy.rent();
       }
   }
   ```

   

### 10.2、动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类时动态生成的，不是我们直接写好的
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - **基于接口----Jdk动态代理**
  - 基于类：cglib
  - java字节码实现：javasist



​	需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序



动态代理的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务；
- 公共业务也就交给代理角色，实现业务的分工；
- 公共业务发生扩张的时候，方便集中管理；
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务；
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可；





# 11.AOP

【重点】使用AOP织入，需要导入一个依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```



方式一：使用Spring的API接口【主要SpringAPI接口实现】

方式二：自定义来实现AOP【主要是切面定义】

![guidao](https://github.com/fakefake00/repo1/raw/master/guidao.jpeg)

![guidao](guidao.jpeg)