---
layout: post
title: GreenDao数据库框架的基础学习
date: 2018-12-17 14:40:28
toc: true
reward: true
tags: [Android,数据库]
---
GreenDao很早就想看看了，最近由于业务需求的变化，我就开始学习下简单的增、删、改、查。
***
## 资料参考：
   - 首先这是官网地址：https://github.com/greenrobot/greendao
   - 参考博客为：https://www.cnblogs.com/wjtaigwh/p/6394288.html
   - 慕课网上的视频教程（有点老了，个人觉得）
***
<!--more-->
### 一、咱们先来学习下GreenDao的Api属性：
> **GreenDao是使用ORM（Object RelationShop Mapping）对象关系映射，就是通过GreenDao将数据库和Bean关联起来有以下优点：**

- 存取速度快
- 支持数据库加密
- 轻量级
- 激活实体
- 支持缓存
- 代码自动生成
### 二、代码的接入配置
 -  app项目目录下build文件中添加
```
apply plugin: 'org.greenrobot.greendao'
Android{
   //greendao配置
    greendao{
       //版本号，升级时可配置
       schemaVersion 1
    }
}

dependencies {
    implementation 'org.greenrobot:greendao:3.2.2' // add library
    implementation 'org.greenrobot:greendao-generator:3.2.2'
}

```
- 在更目录中build文件中需要添加
```
dependencies {
   classpath 'org.greenrobot:greendao-gradle-plugin:3.2.2'
}
```
#### 预览效果：

![](https://user-gold-cdn.xitu.io/2018/8/9/1651c88d14c0e067?w=1036&h=1598&f=png&s=492342 "是不是很丑")

- ### Bean 对象注释的解释
```
@Entity：告诉GreenDao该对象为实体，只有被@Entity注释的Bean类才能被dao类操作
@Id：对象的Id，使用Long类型作为EntityId，否则会报错。(autoincrement = true)表示主键会自增，如果false就会使用旧值
@Property：可以自定义字段名，注意外键不能使用该属性
@NotNull：属性不能为空
@Transient：使用该注释的属性不会被存入数据库的字段中
@Unique：该属性值必须在数据库中是唯一值
@Generated：编译后自动生成的构造函数、方法等的注释，提示构造函数、方法等不能被修改
```
- ### 在Application中初始化自己的数据库
```
    /**
     * 配置数据库
     */
    private void setupDatabase() {
        //创建数据库shop.db
        DaoMaster.DevOpenHelper helper = new DaoMaster.DevOpenHelper(this,"shop.db",null);
        //获取可写数据库
        SQLiteDatabase db = helper.getWritableDatabase();
        //获取数据库对象
        DaoMaster daoMaster = new DaoMaster(db);
        //获取dao对象管理者
        daoSession = daoMaster.newSession();
    }
    public static DaoSession getDaoInstant(){
        return daoSession;
    }
```
- ### 在使用前我们海的封装一个数据库的操作类，提供简单的Shop对象的增删该查的简单方法
```
 * 使用GreenDao 实现简单的增删改查，下面是基本方法
   * 增加单个数据
 * getShopDao().insert(shop);
 * getShopDao().insertOrReplace(shop);
 * 增加多个数据
 * getShopDao().insertInTx(shopList);
 * getShopDao().insertOrReplaceInTx(shopList);
 * 查询全部
 * List< Shop> list = getShopDao().loadAll();
 * List< Shop> list = getShopDao().queryBuilder().list();
 * 查询附加单个条件
 * .where()
 * .whereOr()
 * 查询附加多个条件
 * .where(, , ,)
 * .whereOr(, , ,)
 * 查询附加排序
 * .orderDesc()
 * .orderAsc()
 * 查询限制当页个数
 * .limit()
 * 查询总个数
 * .count()
 * 修改单个数据
 * getShopDao().update(shop);
 * 修改多个数据
 * getShopDao().updateInTx(shopList);
 * 删除单个数据
 * getTABUserDao().delete(user);
 * 删除多个数据
 * getUserDao().deleteInTx(userList);
 * 删除数据ByKey
 * getTABUserDao().deleteByKey();
```
*****我的代码和网上大佬的一样，其实就是自己平时封装的工具类！累。。。。。
那么那么久*****

最后附上自己的练习代码：[Coding](https://coding.net/u/Mark_Chen/p/MarkGreenDaoDemo/git)
在大佬的基础上学习就是快！！！

大佬的博客为：http://www.cnblogs.com/wjtaigwh/

