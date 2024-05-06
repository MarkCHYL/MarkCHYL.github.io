---
layout: post
title: Glide中Bug：宽度铺满高度自适应 & GIF加载之坑
toc: true
reward: true
date: 2023-02-24 10:50:53
tags: [bug]
categories: [Android]
---
## 一、前言：
Glide圆角和centerCrop都是有问题的

1. imageview自带的centerCrop是不管图片小于还是大于imageview的大小，都会等比例拉伸填充满，然后裁剪；
2. 而Glide的centerCrop对于大图是裁剪，如果图片小于imageview，则是等比例全部显示在imageview里而不是填满裁剪;
3. 而且如果imageview自己设置了centeCrop，这时候Glide再设置圆角，如果图片原图小于imageview，圆角是无效的

**Glide 的基本使用**可以查看下面这些文章：

[图片加载库Glide介绍](http://ocnyang.com/2016/08/09/GlideAbout/)

[Glide图片加载库的使用](http://ocnyang.com/2016/08/17/GlideUse/)
<!-- more -->

*** 

## 二、Glide 实现 ImageView 宽度填满，高度自适应的效果
先说一下大家在平时用到 ImageView 实现宽度填满，高度自适应的方法。

> ImageView 宽度填满，高度自适应常用在：
> 1. ListView 列表布局的条目中（RecycleView 同理），比如实现 item 中的图片充满屏幕，高度根据具体图片比例自适应，商品详情中常常用到。
> 2. GridView 网格布局的条目中，假如 item 有两列，想让每一列的 item 中的图片占用屏幕的一半。
> 3. 其他使用单独图片也想达到这种效果的场景。

这里提供两种实现方法:
### 1、重写 onMeasure 方法
```java
@SuppressLint("AppCompatCustomView")
public class ResizableImageView extends ImageView {

    private int value=0;

    public ResizableImageView(Context context) {
        super(context);
    }

    public ResizableImageView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec){
        Drawable d = getDrawable();
        if(d!=null){
            int width = MeasureSpec.getSize(widthMeasureSpec);
            int height = width;
            //高度根据使得图片的宽度充满屏幕计算而得（这个是默认计算）
            //  int height = (int) Math.ceil((float) width * (float) d.getIntrinsicHeight() / (float) d.getIntrinsicWidth());
            if (value==1){
                 height = (int) Math.ceil((float) (width*4/3));
            }
            setMeasuredDimension(width, height);
        }else{
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }
    }

    //根据传过来的值，1是宽度3：4，其它值是1：1
    public void  setMeasure(int value){
        this.value= value;
    }
}
```
### 2、设置 ImageView 的属性
```java
<ImageView
        android:id="@+id/iv_ocnyang"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
        android:scaleType="fitXY"
        />
```
> `fitXY` 这种图片的显示方式的效果是：根据 `ImageView` 设置的大小拉伸图片以填充满空间，（单独设置此属性时）图片会变形。
> 
>`adjustViewBounds` 是限制图片在显示时保持原图比例。（和 `fitXY` 显示方式合用能到达自适应的效果）
***
**通过这上面两种方式显示图片一般都能够宽度充满高度自适应的效果，可是当你用 Glide 请求显示网络图片的时候，你会很失望的发现上面的设置失效了同时图片也变形了。**
***

那么这时候是哪里出了问题了呢？（下面只做一个笼统的分析，具体可以看这个链接： [Glide使用及注意的地方](https://github.com/clarkehe/Android/wiki/Coding(7):-Glide%E4%BD%BF%E7%94%A8%E5%8F%8A%E6%B3%A8%E6%84%8F%E7%9A%84%E5%9C%B0%E6%96%B9)）

其实如果你熟知 Glide 的话，可能你还记得，Glide 在加载图片的时候，加载的大小会和 ImageView 的大小保持一致。也就是 ImageView 的大小决定了 Glide 加载图片的尺寸。而这里我们的 ImageView 设置的高度是 wrap_content，Glide 就无法准确的加载图片的大小了。

那这个时候怎么才能保证按原图的比例来自适应高度显示呢？
这里有两种方式：
1. 你已经知道图片（或其他方式提前知道）图片的比例，然后在用 Glide 请求图片时限制图片的加载大小，即设置 override(int width, int height) 。这时候加载到的图片是原图比例，显示的时候虽然有拉伸/压缩但都会保存原比例的。这种方式适用于你加载的图片大小都比较规范固定的时候。

2. 当然，你请求的图片源并不一定大小都一致。那这时候就可以使用下面这种方式了。这种方式的原理是，先使用 Glide 把图片的原图请求加载过来，然后再按原图来显示图片。
```java
    Glide.with(mContext)
    .load(url)
    .asBitmap()
    .into(new SimpleTarget<Bitmap>() {
    @Override
    public void onResourceReady(Bitmap resource, GlideAnimation<? super Bitmap> glideAnimation) {
    ivOcnyang.setImageBitmap(resource);
    }
    });
````
>这两种方法中，其实更加提倡的是第一种方式，因为这种方式不会造成任何负面的影响。但第二种方式，由于Glide加载图片时是以全分辨率加载的，当加载图片过大且图片很多时，可能造成 OOM。同时第二种方式使用在列表上复用时会造成条目错乱错位。
***

## 三、Glide 加载 Gif 图片的那些坑
![](https://upload-images.jianshu.io/upload_images/2625875-9a044086b7de0a45.gif)

###  1、加载 Gif 图片慢或者显示不出来

这是一个公认的问题了，在 Glide 的 issue 上有人提出过，并且作者也给出了解决方案。
加载 GIF 时需要调用 asGif() 方法，同时设置特别的缓存策略，调用 diskCacheStrategy() 将缓存策略设置为 SOURCE（缓存原图） 或者 NONE（不做缓存）。

>Glide 在加载 GIF 时不调用 asGif() 方法也是能正常显示动画的。但建议调用 asGif()。

```java
if (imgUrl.toUpperCase().endsWith(".GIF")) {
            Glide.with(mContext)
                    .load(imgUrl)
                    .asGif()
                    .override(width, height)
                    .placeholder(placeholderImg)
                    .error(errorImg)
                    .dontAnimate() //去掉显示动画
                    .centerCrop()
                    .diskCacheStrategy(DiskCacheStrategy.SOURCE) //DiskCacheStrategy.NONE
                    .into(ivOcnyang);
        } else {
            Glide.with(mContext)
                    .load(imgUrl)
                    .override(width, height)
                    .placeholder(placeholderImg)
                    .error(errorImg)
                    .crossFade()
                    .centerCrop()
                    .into(ivOcnyang);
        }
```
[原文查看更多](http://events.jianshu.io/p/f05798900e3e)