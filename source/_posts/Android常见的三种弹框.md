---
layout: post
title: Android常见的三种弹框
date: 2019-08-06 09:26:13
tags: [Android]
categories: [Android]
---
再简单的东西时间长了，用的次数少了，会容易忘记。反之再复杂的东西其实也是由普普通通的东西实现的。
**[感谢这位博主](https://blog.csdn.net/qq_35698774/article/details/79779238)总结**。
Android在开发中经常会遇到有弹框的需求。经常使用的有Dialog 弹框，Window弹框，Activity伪弹框这三种。
<!--more-->

我这今天主要是记录下Dialog，简单的交互：
Dialog继承Object,异步调用，不会阻塞UI线程。以下是对他的整体框架：

![图片来自网上](https://img-blog.csdn.net/2018040116510588 "图片来自网上")

我今天使用的就是AlertDialog，以下其API方法：
```
 setTitle ：为对话框设置标题
 setIcon ：为对话框设置图标
 setMessage：为对话框设置内容
 setView ： 给对话框设置自定义样式
 setItems ：设置对话框要显示的一个list，一般用于显示几个命令时
 setMultiChoiceItems ：用来设置对话框显示一系列的复选框
 setSingleChoiceItems ：用来设置对话框显示一系列的单选框
 setNeutralButton    ：普通按钮
 setPositiveButton   ：给对话框添加"Yes"按钮
 setNegativeButton ：对话框添加"No"按钮
 create ： 创建对话框
 show ：显示对话框
 ```

 下面我贴一下我自己的代码：
 ```
 private void showDialog(SlibingMenuActivity mActivity, String message, int i) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(mActivity);
        dialog.setTitle("温馨提示：");
        dialog.setMessage(message);
        dialog.setNegativeButton(R.string.cancel, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                Toast.makeText(mActivity, "您点击了取消", Toast.LENGTH_SHORT).show();
            }
        });
        dialog.setPositiveButton(R.string.ok, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                if (i == 0) {
                    ClockOn();//上班
                } else if (i == 1) {
                    ClockOff();
                } else if (i == 2) {
                    logout();
                }
            }
        });
        dialog.create();
        dialog.show();
    }

```

## 1.只显示标题和内容

```
AlertDialog alertDialog1 = new AlertDialog.Builder(this)
        .setTitle("这是标题")//标题
        .setMessage("这是内容")//内容
        .setIcon(R.mipmap.ic_launcher)//图标
        .create();
alertDialog1.show();
```

![](https://img-blog.csdn.net/20180401170308265)

## 2.有多个按钮

>setPositiveButton  设置一个确定按钮

>setNegativeButton  设置一个取消按钮

>setNeutralButton   设置一个普通按钮
```
这三个按钮可以随意组合使用不冲突。
AlertDialog alertDialog2 = new AlertDialog.Builder(this)
        .setTitle("这是标题")
        .setMessage("有多个按钮")
        .setIcon(R.mipmap.ic_launcher)
        .setPositiveButton("确定", new DialogInterface.OnClickListener() {//添加"Yes"按钮
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "这是确定按钮", Toast.LENGTH_SHORT).show();
            }
        })
 
        .setNegativeButton("取消", new DialogInterface.OnClickListener() {//添加取消
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "这是取消按钮", Toast.LENGTH_SHORT).show();
            }
        })
        .setNeutralButton("普通按钮", new DialogInterface.OnClickListener() {//添加普通按钮
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "这是普通按钮按钮", Toast.LENGTH_SHORT).show();
            }
        })
        .create();
alertDialog2.show();
```

![](https://img-blog.csdn.net/20180401170521672)
 

## 3.一个列表

>setItems  里面写列表数据
```
final String[] items3 = new String[]{"苍老湿", "小泽老湿", "波多野结衣老湿", "吉泽明步老湿"};//创建item
AlertDialog alertDialog3 = new AlertDialog.Builder(this)
        .setTitle("选择您喜欢的老湿")
        .setIcon(R.mipmap.ic_launcher)
        .setItems(items3, new DialogInterface.OnClickListener() {//添加列表
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "点的是：" + items3[i], Toast.LENGTH_SHORT).show();
            }
        })
        .create();
alertDialog3.show();
```
![](https://img-blog.csdn.net/20180401170703518)


## 4.单选列表

>setSingleChoiceItems  单选框列表
```
final String[] items4 = new String[]{"苍老湿", "小泽老湿", "波多野结衣老湿", "吉泽明步老湿"};//创建item
AlertDialog alertDialog4 = new AlertDialog.Builder(this)
        .setTitle("选择您喜欢的老湿")
        .setIcon(R.mipmap.ic_launcher)
        .setSingleChoiceItems(items4, 0, new DialogInterface.OnClickListener() {//添加单选框
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                index = i;
            }
        })
        .setPositiveButton("确定", new DialogInterface.OnClickListener() {//添加"Yes"按钮
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "这是确定按钮" + "点的是：" + items4[index], Toast.LENGTH_SHORT).show();
            }
        })
 
        .setNegativeButton("取消", new DialogInterface.OnClickListener() {//添加取消
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "这是取消按钮", Toast.LENGTH_SHORT).show();
            }
        })
        .create();
alertDialog4.show();
```
 
## 5.多选列表
> setMultiChoiceItems 多选框

```
final String[] items5 = new String[]{"苍老湿", "小泽老湿", "波多野结衣老湿", "吉泽明步老湿"};//创建item
final boolean[] booleans = {true, true, false, false};
AlertDialog alertDialog5 = new AlertDialog.Builder(this)
        .setTitle("选择您喜欢的老湿")
        .setIcon(R.mipmap.ic_launcher)
        .setMultiChoiceItems(items5, booleans, new DialogInterface.OnMultiChoiceClickListener() {//创建多选框
            @Override
            public void onClick(DialogInterface dialogInterface, int i, boolean b) {
                booleans[i] = b;
            }
        })
        .setPositiveButton("确定", new DialogInterface.OnClickListener() {//添加"Yes"按钮
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                StringBuilder stringBuilder = new StringBuilder();
                for (int a = 0; a < booleans.length; a++) {
                    if (booleans[a]) {
                        stringBuilder.append(items5[a] + " ");
                    }
                }
                Toast.makeText(AlertDialogActivity.this, "这是确定按钮" + "点的是：" + stringBuilder.toString(), Toast.LENGTH_SHORT).show();
            }
        })
 
        .setNegativeButton("取消", new DialogInterface.OnClickListener() {//添加取消
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "这是取消按钮", Toast.LENGTH_SHORT).show();
            }
        })
        .create();
alertDialog5.show();
```

![](https://img-blog.csdn.net/20180401170947418)
 

### 6.setAdapter的用法

```
final String[] items6 = new String[]{"苍老湿", "小泽老湿", "波多野结衣老湿", "吉泽明步老湿"};//创建item
AlertDialog alertDialog6 = new AlertDialog.Builder(this)
        .setTitle("选择您喜欢的老湿")
        .setIcon(R.mipmap.ic_launcher)
        .setAdapter(new ArrayAdapter<String>(AlertDialogActivity.this, R.layout.item, R.id.tv, items6), new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(AlertDialogActivity.this, "点的是：" + items6[i], Toast.LENGTH_SHORT).show();
            }
        })
        .create();
alertDialog6.show();
```

![](https://img-blog.csdn.net/20180401171051313)
 

## 7.自定义view的用法

```
final AlertDialog.Builder alertDialog7 = new AlertDialog.Builder(this);
View view1 = View.inflate(this, R.layout.activity_alter_dialog_setview, null);
final EditText et = view1.findViewById(R.id.et);
Button bu = view1.findViewById(R.id.button);
alertDialog7
        .setTitle("Login")
        .setIcon(R.mipmap.ic_launcher)
        .setView(view1)
        .create();
final AlertDialog show = alertDialog7.show();
bu.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(AlertDialogActivity.this, "电话" + et.getText().toString(), Toast.LENGTH_SHORT).show();
        show.dismiss();
    }
});
```

![](https://img-blog.csdn.net/20180401211644887)

[原文链接：](https://blog.csdn.net/qq_35698774/article/details/79779238)