---
layout: post
title: 安全码SHA1如何获取
toc: true
reward: true
date: 2020-01-15 08:57:39
tags: [Android]
categories: [Android]
---
在高德地图集成的时候遇到那玩意。
[原文](https://blog.csdn.net/qq_33704095/article/details/80861146)
### SHA1获取的几种方式
* 1、通过Eclipse编译器获取SHA1

使用 adt 22 以上版本，可以在 eclipse 中直接查看。

Windows：依次在 eclipse 中打开 Window -> Preferances -> Android -> Build。

Mac：依次在 eclipse 中打开 Eclipse/ADT->Preferances -> Android -> Build。

在弹出的 Build 对话框中 “SHA1 fingerprint” 中的值即为 Android 签名证书的 Sha1 值

<!-- more -->
* 2、通过Android Studio编译器获取SHA1

第一步、打开Android Studio的Terminal工具

第二步、输入命令：keytool -v -list -keystore keystore文件路径

第三步、输入Keystore密码

* 3、使用 keytool（jdk自带工具）获取SHA1

第一步、运行进入控制台
第二步、在弹出的控制台窗口中输入 cd .android 定位到 .android 文件夹
第三步、继续在控制台输入命令
debug.keystore：命令为：keytool -list -v -keystore debug.keystore

自定义的 keystore：命令为：keytool -list -v -keystore apk的keystore

提示输入密钥库密码，编译器提供的debug keystore默认密码是 android，自定义签名文件的密码请自行填写。输入密钥后回车（如果没设置密码，可直接回车），此时可在控制台显示的信息中获取 SHA1 值

* 4、代码中提取
publicstatic String sHA1(Context context) {
    try {
        PackageInfo info = context.getPackageManager().getPackageInfo(
            context.getPackageName(), PackageManager.GET_SIGNATURES);
        byte[] cert = info.signatures[0].toByteArray();
        MessageDigest md = MessageDigest.getInstance("SHA1");
        byte[] publicKey = md.digest(cert);
        StringBuffer hexString = new StringBuffer();
        for (int i = 0; i < publicKey.length; i++) {
            String appendString = Integer.toHexString(0xFF & publicKey[i])
                .toUpperCase(Locale.US);
            if (appendString.length() == 1)
                hexString.append("0");
            hexString.append(appendString);
        }
        return hexString.toString();
    } catch (NameNotFoundException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
    }
    return null;
}

* 5、apk中读取：
第一步、将apk改为zip后缀文件，并解压；
第二步、进入META-INF路径，执行keytool -printcert -file META-INF/CERT.RSA