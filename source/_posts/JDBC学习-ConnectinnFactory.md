---
layout: post
title: JDBC学习--ConnectinnFactory
toc: true
reward: true
date: 2021-05-13 14:42:47
tags: [JDBC]
categories: [JavaWeb]
---
[toc]
### ConnectionFactory的作用

* 利用工厂模式提升代码的额重要性
* 封装注册数据库的驱动和获得数据库的连接
* 利用配置文件减少硬编码，便于维护

### ConnectionFactory的开发

* 配置文件 `jdbcinfo.properties`

  ```java
  oracle.driver=oracle.jdbc.driver.OracleDriver  //数据库驱动
  oracle.url=jdbc:oracle:thin:@localhost:1521:helowin //数据库地址 helowin:数据库的名字
  oracle.user=mark  //管理账户
  oracle.password=chenyunlin  //密码
  ```
<!-- more -->
- 配置文件信息的获取

  ```java
  static {
          Properties props = new Properties();
          // 加载配置文件中的数据
          InputStream is =
                  ConnectionFactory.class.getClassLoader().getResourceAsStream("jdbcinfo.properties");
          try {
              props.load(is);
              DRIVER = props.getProperty("oracle.driver");
              URL = props.getProperty("oracle.url");
              USER = props.getProperty("oracle.user");
              PASSWORD = props.getProperty("oracle.password");
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
  ```

- 数据库连接Connection的获取

     * ```java
       public static connection getConnection(){
       
       Connection conn = null;
               try {
                   Class.forName(DRIVER);
                   conn = DriverManager.getConnection(URL, USER, PASSWORD);
               } catch (ClassNotFoundException | SQLException e) {
                   e.printStackTrace();
               }
               return conn;
       
       }
       ```
       > 调用getConnection()方法后，
       Console输出：`oracle.jdbc.driver.T4CConnection@f8c1ddd`  表示数据连接成功