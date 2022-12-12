# MybatisQuestion

### 想利用mybatis实现群作业中的转账功能需要哪几步?(需要你把每一步的功能说清楚)

1. 创建Maven工程,配置pom文件,配置依赖,导入mysql,mybatis,junit驱动
2. 创建Account数据表
``` mysql
CREATE TABLE IF NOT EXISTS account(
		id VARCHAR(255) PRIMARY KEY COMMENT 'id',
		`name` VARCHAR(255) COMMENT '用户名',
		money INT(255) COMMENT '账户余额',
		createtime DATE COMMENT '开户时间',
		updatetime DATE COMMENT '更新时间'
		
);
```
3. 创建Account类,定义各属性,get&set方法,toString方法
``` java

public class Account {
    private String id;
    private String name;
    private Integer money;
    private Date createtime;
    private Date updatetime;
	......
}
```
4. 编写AccountMapper接口
``` java

public interface AccountDao {
    // 1查询所有记录
    List<Account> findAll();
    // 2通过id删除记录
    int deleteByPrimaryKey(String id);
	......
}
```
5. 编写AccountMapper映射文件
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.snbo.dao.AccountDao">
		......
<mapper>
```
6. 编写mybatis核心配置文件
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"/>

    <environments default="e1">
        <environment id="e1">
            <transactionManager type="JDBC"/>

            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <mapper resource="org/snbo/mapper/AccountMapper.xml"/>
    </mappers>
</configuration>
```
7. 创建AccountService类,编写transfer方法,先通过id查询出两个账户代表的实例,修改两个实例的money,再通过实例更新表中数据,最后提交数据关闭资源, (自定义异常类,转账金额大于剩余金额时抛出异常) 异常的处理放在方法外面,只有方法编译通过时才会提交数据,关闭资源
``` java
public void transfer(String remitterId, String remitteeId, int money) throws Exception {
        if (money == -1 || money == 0) {
            destroy();
            return;
        }
        Account account = accountDao.selectByPrimaryKey(remitterId);
        Integer money1 = account.getMoney();
        if (money > money1) {//自定义异常类,转账金额大于剩余金额时抛出异常
            throw new ExcessException(money);
        }
        account.setMoney(money1 - money);

        Account account1 = accountDao.selectByPrimaryKey(remitteeId);
        Integer money2 = account1.getMoney();
        account1.setMoney(money2 + money);

        accountDao.updateByPrimaryKey(account);
        accountDao.updateByPrimaryKey(account1);

        destroy();
    }
```
### Maven项目的target文件夹是干什么的
target是用来存放项目构建后的文件和目录、jar包、war包、编译的class文件
所有都是Maven构建时生成的。
### 我把UserMapper.xml文件放在了Dao这个包下运行后报错,显示找不到这个.xml文件,可能是哪方面的问题,该如何解决?
UserMapper.xml文件需要放到resources资源文件里面,并且在mybatis核心文件中配置好mapper文件路径
### mybatis如何实现模糊查询
使用 concat() 连接函数连接'%'
``` xml
 <where>
        <if test="partNum!=null">
            partNum LIKE CONCAT('%', #{partNum}, '%')
        </if>
    </where>
```
### mybatis分页操作有哪几步
1. 接口中定义方法 , 参数类型设为Map
2. 映射文件里编写对应方法,开始下标和长度名要和Map中键名一致
``` java
某个类 getLimit(Map<String,Integer> map);
```
``` xml
    <select id="getLimit" resultType="返回值类型全类名">
        select * from account limit #{startIndex} , #{size}
    </select>
```
### Lombok注解是干什么的,注解是什么意思,用到了Java的哪个原理
1. Lombok 是一种 Java™ 实用工具,通过注解消除一些样板代码,通过注解自动生成这些代码
2. 注解就像是一种标签,给程序中的类,方法,变量等贴上标签,然后被标签标记的类或方法们会获得一些特殊的待遇,可以进行一些特殊的操作
3. 注解用到了java的反射原理,通过反射获得某个类的注解,在根据其注解进行特定操作
### 一对多 多对一操作怎么实现
1. 一对多操作
   - 在实体类里面定某属性
   - 在接口中定义方法
    ``` java
     List<Account> selectByIds(List<String> list);
    ```
   - 修改Mapper映射文件,通过自定义resultMap,实现特定的封装
   ``` xml
   <mapper namespace="com.snbo.mapper.UserMapper">
       <resultMap id="userMap" type="com.snbo.domain.User">
           <result column="id" property="id"></result>
           <result column="username" property="username"></result>
           <result column="password" property="password"></result>
           <result column="birthday" property="birthday"></result>
           <collection property="orderList" ofType="com.snbo.domain.Order">
               <result column="oid" property="id"></result>
               <result column="ordertime" property="ordertime"></result>
               <result column="total" property="total"></result>
           </collection>
       </resultMap>
       
       <select id="findAll" resultMap="userMap">
           select *,o.id oid from user u left join orders o on u.id=o.uid
       </select>
   </mapper>
   ```
   
2. 多对一操作
   - 与一对一查询没有太大区别
### Mybatis缓存是什么,如何切换一级缓存/二级缓存
1. Mybatis提供查询缓存，用于减轻数据压力，提高数据库性能。当程序要读取数据时，会首先从缓存中获取。通常情况下mybatis会访问数据库获取数据，中间涉及到很多操作，为了加快数据查询的速度，在其内部引入了缓存来加快数据的查询速度。

2. mybatis默认为一级缓存
- 一级缓存切换二级缓存
   - 首先在核心配置文件中开启二级缓存
   ```xml
   <settings>
       <setting name="cacheEnabled" value="true"></setting>
   </settings>
   ```
   - 
   
   ```xml
   <!--在Mapper中加入标签, 或在接口的方法中加入@CacheNamespace注释(一个就好)-->
   <mapper namespace="......">
     <cache></cache>
   ```
   - 对应实例类实现Serializable接口
   ``` java
    public class * implements Serializable{
   ```
### 动态SQL的实现
- if 动态sql ,当if条件成立时才会添加相应sql语句,当where中内容不为空时,添加where语句

``` xml
<select id="findByCondition" parameterType="user" resultType="user">
    select * from User
    <where>
        <if test="id!=0">
            and id=#{id}
        </if>
        <if test="username!=null">
            and username=#{username}
        </if>
    </where>
</select>
```

- foreach 动态sql,用来遍历数组或集合,可用于查询多个值或者批量插入

- collection：代表要遍历的集合元素，注意编写时不要写
  •open：代表语句的开始部分
  •close：代表结束部分
  •item：代表遍历集合的每个元素，生成的变量名
  •sperator：代表分隔符

  ```xml
  <select id="findByIds" parameterType="list" resultType="user">
      select * from User
      <where>
          <foreach collection="array" open="id in(" close=")" item="id" separator=",">
              #{id}
          </foreach>
      </where>
  </select>
  ```

  
