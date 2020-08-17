---
layout: post
title: Mark的Retrofit学习第一篇
date: 2018-12-17 10:30:47
toc: true
reward: true
tags: [Android,网络框架]
---
## Retrofit介绍 :
改造将您的HTTP API变成一个Java界面。Square公司针对Android和Java的类型安全HTTP客户端
       Retrofit是Square公司开发的一款针对Android网络请求的框架，Retrofit2底层基于OkHttp实现的，OkHttp现在已经得到Google官方认可，大量的app都采用OkHttp做网络请求，其源码详见OkHttp Github。

       每个方法都必须有一个提供请求方法和相对URL的HTTP注释。有五个内置注释：GET，POST，PUT，DELETE，和HEAD。资源的相对URL在注释中指定。
<!--more-->
## 一、声明接口
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}

///Retrofit turns your HTTP API into a Java interface.

eg：例子：
如果链接是http://ip.taobao.com/service/getIpInfo.php?ip=202.202.33.33
 @GET("http://ip.taobao.com/service/getIpInfo.php")
    Call getWeather(@Query("ip") String ip);

注意：这里强调一点：Call get();必须是这种形式,这是2.0之后的新形式
 * 如果不需要转换成Json数据,可以用了ResponseBody;
 * 你也可以使用Call get();这样的话,需要添加Gson转换器

## 二、接口调用
Retrofit retrofit = new Retrofit.Builder()
 //这里建议：- Base URL: 总是以/结尾；- @Url: 不要以/开头
    .baseUrl("https://api.github.com/")
    .build();

GitHubService service = retrofit.create(GitHubService.class);

Call<List<Repo>> repos = service.listRepos("octocat");

//The Retrofit class generates an implementation of the GitHubService interface.
强调一点：如果你的@GET后面的URL地址是全拼的地址，那么baseurl就会设置的没有必要，留意哦！！！

## 三、同步调用
  try {
            Response bodyResponse = call.execute();
            String body = bodyResponse.body().string();//获取返回体的字符串
            Log.i("wxl", "body=" + body);
        } catch (IOException e) {
            e.printStackTrace();
注意：同步需要处理android.os.NetworkOnMainThreadException

## 四、异步调用
call.enqueue(new Callback() {
            @Override
            public void onResponse(Response response) {
                try {
                    Log.i("wxl", "response=" + response.body().string());
                } catch (IOException e) {
                    e.printStackTrace();
            @Override
            public void onFailure(Throwable t) {
                Log.i("wxl", "onFailure=" + t.getMessage());
        });

注意这里看不懂的可以参考我的GitHub代码地址：https://github.com/MarkCHYL/MarkRetrofitDemo
项目中添加了简单的使用方法，只做到如何获取网络接口的数据。

## 五、移除请求
call.cancel();

## 六、添加gson依赖
 implementation 'com.squareup.retrofit2:converter-gson:2.3.0'
这里我选择的是与retrofit统一版本的，我怕报错

后续我会进一步封装！！！

## 七、Retrofit2打印 网络请求日志
添加依赖：
 implementation 'com.squareup.okhttp3:logging-interceptor:3.8.1'
## 然后就是你初始化拦截器加配置；这个很简单我参考了以下两篇文章
https://blog.csdn.net/u011511368/article/details/51753315
https://blog.csdn.net/Picasso_L/article/details/53200926

### 最后附上自己的[Github的Demo](https://github.com/MarkCHYL/MarkRetrofitDemo)