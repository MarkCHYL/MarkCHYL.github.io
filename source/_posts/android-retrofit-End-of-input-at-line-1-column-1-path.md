---
layout: post
title: android retrofit End of input at line 1 column 1 path
toc: true
reward: true
date: 2020-09-03 09:14:59
tags: [Error]
categories: [Android]
---
在使用retrofit作为项目的网络请求库时，接口定义如下：
```
@GET(ACT_GET_NEW_STAFF)
Call<TaskEn> reqGetNewStaff();
```
接口从服务端获取了数据，通过GsonConverterFactory将服务端相应内容解析成对应的实体类。在接口正常响应时（有数据返回），并没有什么异常发生，但当接口请求的数据为空，我们的服务端人员并不是返回理论意义上的空，null或者[]（数据集合空），而是返回没有响应体body，只有响应头header，content-length为0的Response

这时候GsonConverterFactory就解析异常了，并抛出如下异常：
<!-- more -->
>java.io.EOFException:End of input at line 1 column 1 path 

一般来说，如果接口本身就是不需要处理body的，那么我们通常定义接口为
```
 Call<Void>
 ```
 这和上面的那两个接口是不一样的。

 ### 解决方案
 * 请服务端人员吃顿饭，让他们规范接口，当数据为空时，返回null或者[]
 * 自己动手丰衣足食
  
  自定义一个ConverterFactory
```
public class NullOnEmptyConverterFactory extends Converter.Factory {

    @Override
    public Converter<ResponseBody, ?> responseBodyConverter(Type type, Annotation[] annotations, Retrofit retrofit) {
        final Converter<ResponseBody, ?> delegate = retrofit.nextResponseBodyConverter(this, type, annotations);
        return new Converter<ResponseBody,Object>() {
            @Override
            public Object convert(ResponseBody body) throws IOException {
                if (body.contentLength() == 0) return null;
                return delegate.convert(body);
            }
        };
    }
}
```
然后设置到retrofit
```
Retrofit retrofit = new Retrofit.Builder()
    ....
    .addConverterFactory(new NullOnEmptyConverterFactory())
    .addConverterFactory(GsonConverterFactory.create())
    .build();
```
需要注意的是，NullOnEmptyConverterFactory必需在GsonConverterFactory之前addConverterFactory

[原文连接](http://www.voidcn.com/article/p-xoiqdiuz-re.html)