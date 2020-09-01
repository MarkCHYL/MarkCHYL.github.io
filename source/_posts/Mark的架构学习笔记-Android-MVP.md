---
layout: post
title: Mark的架构学习笔记---Android_MVP
date: 2018-12-17 14:43:22
toc: true
reward: true
tags: [Android,架构]
categories: [Android]
---
# 前言：
   我是名安卓程序员。当我第一次接触到架构这个概念时，说实话我一头雾水。我现实了解了MVC模式，当我开始接触MVP模式时，我啊真的彻底被搞懵了！怎么把一个activity拆成了四个或五者，如果分装过后会更多，想象下，一下子猛的看那么复杂的对我来说无益处。这边文章呢我实现只是一个加你简单单的Demo练习，参考代码和资料来源于网上（备注于文底部）。
   记录只是让自己加深理解。
<!--more-->
![这是我的Demo代码架构](https://user-gold-cdn.xitu.io/2018/7/30/164e8dc7b3e27bb6?w=696&h=812&f=png&s=257193 "这是我的Demo代码架构")
****
+ ## 跟着大佬学习下MVP模式：
   MVP是模型（Model）、视图（View）、主持人（Presenter）的缩写，分别代表项目中3个不同的模块。

![](https://user-gold-cdn.xitu.io/2018/7/30/164e9021bf594592?w=728&h=618&f=png&s=38130 "MVP模式示意图")
说明：
   *   步骤1：UI实现View方法，引用Presenter
   *   步骤2：Presenter调用Model，走Model具体逻辑
   *   步骤3：Model逻辑实现，回调Presenter方法
   *   步骤4：Presenter回调View，即回到UI，回调View方法
   ***                          
## 首先我们定义下View层
   定义接口 **IUserView**
```
public interface IUserView {
    UserBean getUser();
    void setUser(UserBean bean);
}
```
  一个是拿到数据，一个是把数据给别人，
  我们需要让我们的Activity去实现这个接口里面的方法，
  然后去做一些数据的显示或者获取。 Presenter与View交互是通过接口。所以我们这里需要定义一个IUserView，
  难点就在于应该有哪些方法，我们看一眼效果图会发现一个是保存，一个是载入，
  所以我们就创建两个方法，分别是：getUser和setUser；
  然后再MainActivity中与实现这个View层的接口
  如图所示：
  ```
  public class MainActivity extends AppCompatActivity implements IUserView {
    private UserPresenter presenter;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        presenter = new UserPresenter(this);
        initEvent();
    }
    private void initEvent() {
        findViewById(R.id.btn_load).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                presenter.loadUser();
            }
        });
        findViewById(R.id.btn_save).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                presenter.saveUser();
            }
        });
    }
    @Override
    public UserBean getUser() {
        int id = Integer.valueOf(et_id.getText().toString().trim());
        String name = et_name.getText().toString().trim();
        if (name != null) {
            UserBean bean = new UserBean();
            bean.setId(id);
            bean.setName(name);
            return bean;
        }
        return null;
    }
    @Override
    public void setUser(UserBean bean) {
        if (bean!=null){
            tv_data_show.setText(bean.toString());
        }
    }
}
//省略了部分控件的初始化代码
  ```
  是不是觉得曾在某个文章中看见过，哈哈哈，自己敲一边你会得到自己的理解。
***
## 定义下Model层
   * 首先我们的更具接口或者你的业务代码去定义自己的实体类 **UserBean**
   ```
   public class UserBean {
    private int id;
    private String name;
    
    ...getter and setter 方法
    
    @Override
    public String toString() {
        return "UserBean{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```
   * 实体类的接口 IUser
>这里大家要注意一下，IUser里面主要是接口，首先，我们要想好，在Presenter中要实现哪些逻辑，要用到哪些方法，然后就在这里定义哪些方法。User主要是IUser的实现，返回一些数据，具体返回那些数据，就有大家自己去根据实际情况而定。
   ```
   public interface IUser {
    void savaUserInfo(UserBean user);
    UserBean loadUserInfo();
}
   ```
   * 实体类的业务实现类 **User**
>这里主要是实现**IUser**接口里面定义的业务实现方法，你想干啥干就可以在里面自己凿凿
   ```
   public class User implements IUser {
    private UserBean bean;
    @Override
    public void savaUserInfo(UserBean user) {
        this.bean = user;
    }
    @Override
    public UserBean loadUserInfo() {
        if (bean != null) {
            return bean;
        }
        return null;
    }
}
   ```
## 定义最重要的Presenter层
>是连接Activity(在这可以理解为就是View层，因为Activity实现了 **IUserView** 接口)和Model的重要桥梁，所有的业务逻辑都在它里面完成：
```
public class UserPresenter {
    private IUser user;
    private IUserView userView;
    public UserPresenter(IUserView userView) {
        this.userView = userView;
        user = new User();
    }
    public void saveUser(){
        user.savaUserInfo(userView.getUser());
    }
    public void loadUser(){
        userView.setUser(user.loadUserInfo());
    }
}
```
[源码地址](https://github.com/MarkCHYL/Mark_MVP_Sample "源码地址")

下一篇我会结合自己封装的retrofit写一个网络请求的demo笔记，加油⛽️
