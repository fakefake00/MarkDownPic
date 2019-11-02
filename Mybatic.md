# 1、简介

### 1.1、什么是 MyBatis？

1. MyBatis 是一款优秀的**持久层**框架，它支持定制化 SQL、存储过程以及高级映射。
2. MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
3. MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。 

### 1.2、如何获得Mybatis：

- Github：https://github.com/mybatis/mybatis-3/releases

- 中文文档：https://mybatis.org/mybatis-3/zh/index.html

- maven仓库:

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.3</version>
  </dependency>
  
  ```

### 1.3、持久层

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：断电即失
- 数据库（JDBC）、io文件持久化
- 生活：冷藏，罐头

为什么需要持久化？

- 有一些对象，不能让他丢掉
- 内存昂贵

### 1.4、持久层

Dao层、Serviceceng、Controllerceng

- 完成持久化工作的代码块
- 层界限十分明显

### 1.5、为什么需要Mybatis？

- 方便
- 简化传统JDBC代码，框架，自动化
- 不用Mybatis也可以
- 优点：
  - 简单易学
  - 灵活
  - 解除sql与程序代码的耦合，提高可维护性
  - 提供映射标签，支持对象与数据库的orm字段关系映射
  - 提供对象关系映射标签，支持对象关系组建维护
  - 提供xml标签，支持编写动态sql。



# 2、第一个Mybatis程序

思路：搭建环境

