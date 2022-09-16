---
layout: post
title: 实现本地资源包统一管理nexus
toc: true
reward: true
date: 2022-09-16 09:54:42
tags: [Android]
categories: [Android]
---
通过终端命令 cd /Users/xxx/Downloads/nexus-3.14.0-04-mac/nexus-3.14.0-04/bin进入bin目录下，执行
./nexus start
./nexus status


上传示例：

apply plugin: 'maven'

//打包main目录下代码和资源的task
task androidSourcesJar(type: Jar) {
    classifier = 'sources'
    from android.sourceSets.main.java.srcDirs
}
//配置需要上传到maven仓库的文件
artifacts {
    archives androidSourcesJar
}
//上传到Maven仓库的task
uploadArchives {
    repositories {
        mavenDeployer {
            //指定maven仓库url
            repository(url: "http://127.0.0.1:8081/nexus/content/repositories/releases/") {
                //nexus登录默认用户名和密码
                authentication(userName: "admin", password: "admin123")
            }
            pom.groupId = "com.example.as.custom"// 唯一标识（通常为模块包名，也可以任意）
            pom.artifactId = "CustomWidget" // 项目名称（通常为类库模块名称，也可以任意）
            pom.version = "1.1.0" // 版本号
        }
    }
}

3.使用：
和使用本地仓库依赖一样，我们告诉gradle依赖包仓库的位置，在项目根目录下build.gradle中添加：

allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'http://127.0.0.1:8081/nexus/content/repositories/releases/' }
    }
}

然后在app模块build.gradle中添加依赖编译运行成功：