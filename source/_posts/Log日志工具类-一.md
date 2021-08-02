---
layout: post
title: Log日志工具类(一)
toc: true
reward: true
date: 2021-04-07 09:05:54
tags: [Utils]
categories: [Android]
---
实现的效果能将json字符串以格式化的方式显示
```
/**
 * $desc$ Log日志工具类
 *
 * @Author mark
 * @Date 2018/11/26
 */
public class LogUtils {
    //是否Debug
    public static final boolean isMarkDebug = true;

    public static final String LINE_SEPARATOR = System.getProperty("line.separator");
    public static void printLog(String name,String strContent){
        if (isMarkDebug){
            Log.d(name,strContent);
        }
    }
    public static void printLine(String tag, boolean isTop) {
        if (isTop) {
            Log.d(tag, "╔═══════════════════════════════════════════════════════════════════════════════════════");
        } else {
            Log.d(tag, "╚═══════════════════════════════════════════════════════════════════════════════════════");
        }
    }
    public static void printJson(String tag, String msg, String headString) {

        String message;

        try {
            if (msg.startsWith("{")) {
                JSONObject jsonObject = new JSONObject(msg);
                message = jsonObject.toString(4);//最重要的方法，就一行，返回格式化的json字符串，其中的数字4是缩进字符数
            } else if (msg.startsWith("[")) {
                JSONArray jsonArray = new JSONArray(msg);
                message = jsonArray.toString(4);
            } else {
                message = msg;
            }
        } catch (JSONException e) {
            message = msg;
        }

        printLine(tag, true);
        message = headString + LINE_SEPARATOR + message;
        String[] lines = message.split(LINE_SEPARATOR);
        for (String line : lines) {
            Log.d(tag, "║ " + line);
        }
        printLine(tag, false);
    }
}

```
