# Mybatis 笔记 (Kotlin)

## 环境

|    JAVA    | java version "1.8.0_241" |
| :--------: | :----------------------: |
|  MYBATIS   |          3.5.4           |
|   KOTLIN   |          1.3.71          |
|   MYSQL    |          8.0.19          |
| MYSQL JDBC |          8.0.19          |

## 入门

### 快速开始

配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/test?useUnicode=true;characterEncoding=utf8;serverTimezone=UTC;allowPublicKeyRetrieval=true"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="mapper.xml"/>
    </mappers>
</configuration>
```

mapper.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="tech.openEdgn.test.UserEntityMapper">
    <select id="selectUser" resultType="tech.openEdgn.test.UserEntity">
        select * from user_table where id = #{id}
    </select>
</mapper>

```

sql

```mysql
CREATE DATABASE IF NOT EXISTS test;
USE test;
CREATE TABLE IF NOT EXISTS user_table(
    id INT(10) PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL ,
    age INT(10) NOT NULL ,
    last_name VARCHAR(255) NOT NULL ,
    email VARCHAR(255) NOT NULL ,
    register_date BIGINT NOT NULL
);
```

UserEntity.kt

```kotlin
data class UserEntity(
        var id: Int,
        var name: String,
        var age: Int,
        var last_name: String,
        var email: String,
        var registerDate: Long
)
```

main.kt

```kotlin
import org.apache.ibatis.io.Resources
import org.apache.ibatis.session.SqlSessionFactoryBuilder
import java.io.InputStream


fun main() {
    val inputStream: InputStream = Resources.getResourceAsStream("mybatis.xml")
    val sqlSessionFactory = SqlSessionFactoryBuilder().build(inputStream)
    val session = sqlSessionFactory.openSession()
    try {
        val one = session.selectOne<UserEntity>("selectUser", 1)
        println(one)

    } finally {
        session.close()
    }
}
```

### 配置

```xml
<configuration>
    <properties resource="mybatis.properties">

    </properties>
    <environments default="development">
        <environment id="development">
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
        <mapper resource="mapper.xml"/>
    </mappers>
</configuration>
```

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test?useUnicode=true;characterEncoding=utf8;serverTimezone=UTC;allowPublicKeyRetrieval=true
jdbc.username=root
jdbc.password=123456
```

### 设置

支持多数据库

```xml
<!-- 为数据库标识起别名-->
    <databaseIdProvider type="DB_VENDOR">
        <property name="MYSQL" value="mysql"/>
        <property name="Oracle" value="oracle"/>
    </databaseIdProvider>
```

mapper.xml

 ```xml
 <!--指定databaseid -->
      <select id="getUserById" resultType="user" databaseId="mysql">
        select * from user_table where id = #{id}
    </select>
    <select id="getUserById" resultType="user" databaseId="oracle">
        select * from user_table where id = #{id}
    </select>
 ```
