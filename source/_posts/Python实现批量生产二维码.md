---
layout: post
title: Python实现批量生产二维码
toc: true
reward: true
date: 2019-10-18 10:11:29
tags: [Python]
categories: [Python]
---
### 工具库：
* pip3 install pillow
* pip3 install qrcode
* pip3 install xlrd
具体是什呢？再次不做介绍，百度官网查询。
<!--more-->
### 记录编码：
一、读取Excel表哥数据：
```
def getInfo():
    try:
        data = xlrd.open_workbook(r"/Users/mark/Desktop/carlistinfo.xlsx")
        sheetname = "Sheet1"
        table = data.sheet_by_name(sheetname)
        col_values = table.col_values(1)
        return col_values
    except Exception as e:
        print(e)
```
二、生成二维码保存指定文件夹:
```
    matchList = getInfo()
    for matchCode in matchList:
        # 要放在循环里，否则 qr.add_data(filename) 会一直累加
        qr = qrcode.QRCode(version=1, error_correction=qrcode.constants.ERROR_CORRECT_L, box_size=10, border=1)
        filename = matchCode
        qr.add_data(filename)
        qr.make(fit=True)
        img = qr.make_image()
        file = "/Users/mark/Desktop/qrimg"+"/{0}.png".format(matchCode)
        img.save(file)
```

三、执行以上操作：

*`getImage()`*

### 全部源码：
```
import qrcode
import xlrd

def getInfo():
    try:
        data = xlrd.open_workbook(r"/Users/mark/Desktop/carlistinfo.xlsx")
        sheetname = "Sheet1"
        table = data.sheet_by_name(sheetname)
        col_values = table.col_values(1)
        return col_values
    except Exception as e:
        print(e)

def getImage():
    matchList = getInfo()
    for matchCode in matchList:
        # 要放在循环里，否则 qr.add_data(filename) 会一直累加
        qr = qrcode.QRCode(version=1, error_correction=qrcode.constants.ERROR_CORRECT_L, box_size=10, border=1)
        filename = matchCode
        qr.add_data(filename)
        qr.make(fit=True)
        img = qr.make_image()
        file = "/Users/mark/Desktop/qrimg"+"/{0}.png".format(matchCode)
        img.save(file)

getImage()
```