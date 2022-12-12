# spring
## Spring IOC
1. IOC 概念

	- ioc(控制反转) : 把创建对象和对象之间的调用过程交给Spring管理,
	- 目的 : 降低耦合度
2. IOC 底层原理

	- xml解析, 工厂模式, 反射
	- IOC 思想基于 IOC 容器完成,IOC 容器底层就是对象工厂
3. Spring的优势

	方便解耦，简化开发
	通过 Spring 提供的 IoC容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造成的过度耦合。
	用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。
4. Spring 提供 IOC 容器实现的两种方式 : (两个接口)

	- BeanFactory : IOC 容器基本实现,是 Spring 内部使用的接口
	- ApplicationContext : BeanFactory接口的字接口,提供更多功能,一般开发人员使用

## Spring程序开发步骤

 ① 导入 Spring 开发的基本包坐标

 ② 编写 Dao 接口和实现类

 ③ 创建 Spring 核心配置文件

 ④ 在 Spring 配置文件中配置 UserDaoImpl

 ⑤ 使用 Spring 的 API 获得 Bean 实例

## Spring xml管理开发
1. ##### bean标签属性
   
   ​	- (1)id属性 : Bean实例在Spring容器中的唯一标识
   ​	- (2)class属性 : Bean的全限定名称(类的全路径)
   ​	- (3)scope属性 : 指对象的作用范围,范围如下:
   ​	  <img src="C:\Users\sunbo.2000\AppData\Roaming\Typora\typora-user-images\image-20220205203330136.png" alt="image-20220205203330136" style="zoom: 67%;" />
   ​		- 1）当 scope 的取值为singleton时
   ​		  - Bean的实例化个数：1个
   ​		  - Bean的实例化时机：当Spring核心文件被加载时，实例化配置的Bean实例
   ​		  - Bean的生命周期：
   ​		  				 对象创建：当应用加载，创建容器时，对象就被创建了
   ​		  		 对象运行：只要容器在，对象一直活着
   ​		  		 对象销毁：当应用卸载，销毁容器时，对象就被销毁了
   ​		- 2）当 scope 的取值为prototype时
   ​		  - Bean的实例化个数：多个
   ​		  - Bean的实例化时机：当调用getBean()方法时实例化Bean
   ​		  - Bean的生命周期：
   ​		  				 对象创建：当使用对象时，创建新的对象实例
   ​		  		 对象运行：只要对象在使用中，就一直活着
   ​		  		 对象销毁：当对象长时间不用时，被Java 的垃圾回收器回收了
   ​	-  (4）init-method：指定类中的初始化方法名称
   ​	-  (5）destroy-method：指定类中销毁方法名称
   
2. ##### Bean实例化三种方式

    1). 无参构造方法实例化
	
     ```xml
     <!--它会根据默认无参构造方法来创建类对象，如果bean中没有默认无参构造函数，将会创	 建失败-->
     <bean id="userDao" class="org.snbo.dao.impl.UserDaoImpl"/>
     ```

    2). 工厂静态方法实例化

    ```xml
     <!--工厂的静态方法返回Bean实例-->
    <bean id="userDao" class="org.snbo.factory.StaticFactoryBean"
    factory-method="createUserDao" />
    ```
    3). 工厂实例方法实例化

    ```xml
    <!--工厂的静态方法返回Bean实例-->
     <bean id="factoryBean" class="org.snbo.factory.DynamicFactoryBean"/>
     <bean id="userDao" factory-bean="factoryBean" factory- method="createUserDao"/>
    ```
    
3. ##### Bean的依赖注入概念
   
    ```
    依赖注入（Dependency Injection）：它是 Spring 框架核心 IOC 的具体实现。
    在编写程序时，通过控制反转，把对象的创建交给了 Spring，但是代码中不可能出现没有依赖的情况。 IOC 解耦只是降低他们的依赖关系，但不会消除。例如：业务层仍会调用持久层的方法。
    那这种业务层和持久层的依赖关系，在使用 Spring 之后，就让 Spring 来维护了。简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。
    ```
    
4. ##### Bean的依赖注入方式
   
    ```
    1). set方法
    2). 构造方法
    ```
    1). set方法注入
      配置Spring容器调用set方法进行注入
      ```xml
      <bean id="userDao" class="org.snbo.dao.impl.UserDaoImpl"/>
      <bean id="userService" class="org.snbo.service.impl.UserServiceImpl"> 
          <!--name为set的方法名,ref为注入的bean id-->
          <property name="userDao" ref="userDao"/> 
      </bean>
      ```
    2). 构造方法注入
      创建有参构造
      ```xml
      <bean id="userDao" class="org.snbo.dao.impl.UserDaoImpl"/>
      <bean id="userService" class="org.snbo.service.impl.UserServiceImpl"> 
      <!--name是构造方法参数名,ref是要注入的bean id-->
      <constructor-arg name="userDao" ref="userDao"></constructor-arg>
      </bean>
      ```
    
    3). 注入普通数据类型
       ```xml
       <bean ......>
       <property name="name" value="sunbo"/>
       <property name="age" value="20"/>
       </bean>
       ```
    
    4). 注入集合数据类型
      String类型注入list
    
      ```xml
      <bean id="userDao" class="org.snbo.dao.impl.UserDaoImpl">
         <property name="strList">
             <list>
               <value>aaa</value>
               <value>bbb</value>
               <value>ccc</value>
             </list>
         </property>
      </bean>
      ```
      引用类型注入list
      ```xml
      <bean id="u1" class="org.snbo.domain.User"/>
      <bean id="u2" class="org.snbo.domain.User"/>
      <bean id="userDao" class="org.snbo.dao.impl.UserDaoImpl">
         <property name="userList">
           <list>
              <ref bean="u1"/>
              <ref bean="u2"/>
           </list>
         </property>
      </bean>
      ```
      map注入
      ```xml
      <bean id="u1" class="org.snbo.domain.User"/>
      <bean id="u2" class="org.snbo.domain.User"/>
      <bean id="userDao" class="org.snbo.dao.impl.UserDaoImpl">      
          <property name="userMap">
            <map>
              <entry key="user1" value-ref="u1"/>
              <entry key="user2" value-ref="u2"/>
            </map>
          </property>
      </bean>
      ```
    
5. ##### 引入其他配置文件（分模块开发）
   
   - 实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载
   ```xml
   <import resource="applicationContext-xxx.xml"/>
   ```
   
6. ##### bean 的生命周期 

   （1）通过构造器创建 bean 实例（无参数构造）。

   （2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）。

   （3）调用bean的初始化的方法（需要进行配置初始化的方法）。

   （4）bean可以使用了（对象获取到了）。

   （5）当容器关闭时候，调用bean的销毁的方法 (需要进行配置销毁的方法)。

## Spring配置数据源

1. 数据源的开发步骤

   ```
   1.导入数据源的坐标和数据库驱动坐标
   2.创建数据源对象
   3.设置数据源的基本连接数据
   4.使用数据源获取连接资源和归还连接资源
   ```

   1). 导入数据源的坐标和数据库驱动坐标
   ```xml
   <!--C3P0连接池-->
   <dependency>
       <groupId>c3p0</groupId>
       <artifactId>c3p0</artifactId>
       <version>0.9.1.2</version>
   </dependency>
   
   <!--Druid连接池-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.10</version>
   </dependency>
   
   <!--mysql驱动-->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.39</version>
   </dependency>
   ```
   2). 创建连接池
   ```java
   public class Test{
       private DruidDataSource dataSource;
       
       public void setDataSource(DruidDataSource dataSource){
           this.dataSource = dataSource;
       }
   } 
   ```
   
   3). Spring配置数据源
      1). 加载jdbc.properties配置文件获得连接信息,需要引入context命名空间和约束路径：
   
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation=
                         "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
       
       ......
       
   </beans>
   ```
   
      2). properties文件
   
   ```properties
   jdbc.url=jdbc:mysql://localhost:3306/test?
   useSSL=false&serverTimezone=UTC&useUnicode=true&characterEncoding=UTF-8
   jdbc.username=root
   jdbc.password=root
   jdbc.driverClassName=com.mysql.cj.jdbc.Driver
   ```
   
      3). 配置数据源
   
   ```xml
   <!--Spring容器加载properties文件-->
   <context:property-placeholder location="classpath:jdbc.properties"/>
   
   <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="jdbcUrl"         value="${jdbc.url}"/>
        <property name="username"        value="${jdbc.username}"/>
        <property name="password"        value="${jdbc.password}"/>
   </bean>
   ```
   
   


## ApplicationContext的实现类

1. ApplicationContext的实现类
    1). ClassPathXmlApplicationContext
         它是从类的根路径下加载配置文件 推荐使用这种
    2). FileSystemXmlApplicationContext
         它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。
    3). AnnotationConfigApplicationContext
         当使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

2. getBean()方法使用
    其中，当参数的数据类型是字符串时，表示根据Bean的id从容器中获得Bean实例，返回是 Object，需要强转。
    当参数的数据类型是Class类型时，表示根据类型从容器中匹配Bean实例，当容器中相同类型的Bean有多个时， 则此方法会报错。
    
    ```java
    ApplicationContext app = newClasspathXmlApplicationContext("xml文件");
    //app.getBean("id");
    //app.getBean(Class);
    app.getBean("id",XX.class);
    ```

## Spring注解开发
1. Spring原始注解
   Spring原始注解主要是替代<Bean>的配置

   | @Component            | 使用在类上用于实例化Bean                          |
   |-----------------------|-------------------------------------------- --|
   | @Controller          | 使用在web层类上用于实例化Bean                      |
   | @Service             | 使用在service层类上用于实例化Bean                |
   | @Repository          | 使用在dao层类上用于实例化Bean                    |
   | @Autowired           | 使用在字段上用于根据类型依赖注入                 |
   | @Qualifier           | 结合@Autowired一起使用用于根据名称进行依赖注入  |
   | @Resource            | 相当于@Autowired+ @Qualifier，按照名称进行注入 |
   | @Value               | 注入普通属性                                     |
   | @Scope               | 标注Bean的作用范围                               |
   | @PostConstruct       | 使用在方法上标注该方法是Bean的初始化方法         |
   | @PreDestroy          | 使用在方法上标注该方法是Bean的销毁方法           |

2. Spring原始注解应用实例
      1). 使用@Compont或@Repository标识UserDaoImpl需要Spring进行实例化。
      ```java
      //@Component("userDao")
      @Repository("userDao")
      public class UserDaoImpl implements UserDao {
        @Override
        public void save() {
          System.out.println("测试... ...");
        }
      }
      ```
      2). 使用@Compont或@Service标识UserServiceImpl需要Spring进行实例化
      
      ```java
        //@Component("userService")
        @Service("userService")
        public class UserServiceImpl implements UserService {
            //@Autowired
            //@Qualifier("userDao")
            @Resource(name="userDao")
            private UserDao userDao;
            @Override
            public void save() {
                userDao.save();
            }
        }
      ```
      3). 使用@Autowired或者@Autowired+@Qulifier或者@Resource进行userDao的注入
      ```java
          @Repository("userDao")
          public class UserDaoImpl implements UserDao {
              @Value("注入普通数据")
              private String str;
              @Value("${jdbc.driver}")
              private String driver;
              @Override
              public void save() {
                System.out.println(str);
                System.out.println(driver);
                System.out.println("save running... ...");
              }
          }
   ```
   
      4). 使用@Scope标注Bean的范围
   
      ```java
          //@Scope("prototype")
          @Scope("singleton")
          public class UserDaoImpl implements UserDao {
             ......
       }
      ```
   
      5). 使用@PostConstruct标注初始化方法，使用@PreDestroy标注销毁方法
      ```java
          @PostConstruct
          public void init(){
              System.out.println("初始化方法....");
          }
          @PreDestroy
          public void destroy(){
              System.out.println("销毁方法.....");
          }
      ```
   
3. Spring新注解

   ```
   使用上面的注解还不能全部替代xml配置文件，还需要使用注解替代的配置如下：
   1.非自定义的Bean的配置：<bean>
   2.加载properties文件的配置：<context:property-placeholder>
   3.组件扫描的配置：<context:component-scan>
   4.引入其他文件：<import>
   ```
   | 注解 | 说明 |
   |---------------------------|------------------------------------------------------------|
   | @Configuration | 用于指定当前类是一个Spring 配置类，当创建容器时会从该类上加载注解 |
   | @ComponentScan   | 用于指定Spring 在初始化容器时要扫描的包。<br>作用和在Spring 的xml 配置文件中的<br><context:component-scan base-package="com.itheima"/>一样 |
   | @Bean            | 使用在方法上，标注将该方法的返回值存储到 Spring 容器中,运用在配置类中  |
   | @PropertySource  | 用于加载.properties 文件中的配置                                   |
   | @Import          | 用于导入其他配置类                                                 |

4. Spring新注解实例
   1). @Configuration
        @ComponentScan
        @Import
   
   ```java
   @Configuration
   @ComponentScan("org.snbo")
   @Import({DataSourceConfiguration.class})
   public class SpringConfiguration {
   ......
   }
   ```
   2). @PropertySource
        @value
   ```java
   @Configuration
   @PropertySource("classpath:jdbc.properties")
   public class DataSourceConfiguration {
       @Value("${jdbc.driver}")
       private String driver;
       @Value("${jdbc.url}")
       private String url;
       @Value("${jdbc.username}")
       private String username;
       @Value("${jdbc.password}")
       private String password;
       
       ......
   }
   ```
   3). @Bean
   ```java
   @Configuration
   @ComponentScan("org.snbo")
   public class DataSourceConfiguration {
     ......
       
     @Bean(name="dataSource")
     public DataSource getDataSource() throws PropertyVetoException {
         ComboPooledDataSource dataSource = new ComboPooledDataSource();
         dataSource.setDriverClass(driver);
         dataSource.setJdbcUrl(url);
         dataSource.setUser(username);
         dataSource.setPassword(password);
         return dataSource;
     }
   }
   ```
5. AnnotationConfigApplicationContext
   - 当使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。
   ```java
   ApplicationContext app = new AnnotationConfigApplicationContext(SpringConfiguration.class);
   //app.getBean("id");
   //app.getBean(Class);
   app.getBean("id",XX.class);
   ```

## Spring 集成Junit

1. spring 集成 Junit 步骤
   ```
   1. 导入spring集成Junit的坐标
   
   2. 使用@Runwith注解替换原来的运行期
   
   3. 使用@ContextConfiguration指定配置文件或配置类
   
   4. 使用@Autowired注入需要测试的对象
   
   5. 创建测试方法进行测试
   ```

   1). 导入spring集成Junit的坐标
   ```xml
   <!--此处需要注意的是，spring5 及以上版本要求junit 的版本必须是4.12 及以上-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-test</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
       <scope>test</scope>
   </dependency>
   ```

   2). 使用@Runwith注解替换原来的运行期
   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   public class SpringJunitTest {
   ......
   }
   ```
   3). 使用@ContextConfiguration指定配置文件或配置类
   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   //加载spring核心配置文件
   //@ContextConfiguration(value = {"classpath:applicationContext.xml"})
   //加载spring核心配置类
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {
   ......
   }
   ```
   4). 使用@Autowired注入需要测试的对象
   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {
       @Autowired
       private UserService userService;
   }
   ```

   5). 创建测试方法进行测试
   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {
       @Autowiredprivate 
       UserService userService;
       
       @Test
       public void testUserService(){
           userService.save();
       }
   }
   ```

## Spring AOP

1. ##### 概念解释

  AOP(Aspect Oriented Programming),面向切面编程,将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。可用于权限认证、日志、事务处理。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。通俗来讲:**不通过修改源码的方式,在主干功能里面添加新的功能**(动态代理)

 

2. ##### 动态代理
   
   1. spirng aop 就是通过动态代理,在一个切点执行之前或之后对它进行一些操作,这个切点就是一个方法,这些方法所在的类被代理,在代理过程中切入了一些操作.
   
   1). 有接口的情况，使用JDK动态代理。创建接口实现类代理对象，实现代理
   
   ```java
   public class Test {
       public static void main(String[] args) {
           SuperMan superMan = new SuperMan();
           Man man = (Man) ProxyFactory.getProxyInstance(superMan);
           String believe = man.believe();
           System.out.println(believe);
           man.eat("凉皮");
       }
   }
   /*
   接口
   */
   interface Man{
       void eat(String food);
       String believe();
   }
   /*
   被代理类
   */
   class SuperMan implements Man{
   
       @Override
       public void eat(String food) {
           System.out.println("陕西人吃"+food);
       }
   
       @Override
       public String believe() {
           return "I Can Fly!";
       }
   }
   
   class ProxyFactory {
   
       public static Object getProxyInstance(Object obj){
           MyInvocationHandler handler = new MyInvocationHandler();
           handler.setObject(obj);
            
           return Proxy.newProxyInstance(obj.getClass().getClassLoader(),
                                         obj.getClass().getInterfaces(),handler);
       }
   }
   
   class MyInvocationHandler implements InvocationHandler{
       public Object object;
   
       public void setObject(Object object) {
           this.object = object;
       }
   
       /**
        * @param proxy 是代理类实例
        * @throws Throwable
        */
       @Override
       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
           //在方法执行之前插入一些操作,进行一些处理但不用对代理类方法进行改动,
           Object o = method.invoke(object, args);
           return o;
       }
   
   ```
   
   2). 没有接口的情况,使用CGLIB动态代理,创建子类的代理对象,实现代理
