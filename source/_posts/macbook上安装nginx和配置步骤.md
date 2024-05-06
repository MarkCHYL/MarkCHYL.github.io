---
layout: post
title: macbook上安装nginx和配置步骤
toc: true
reward: true
date: 2023-07-27 11:01:15
tags: [Nginx]
categories: [服务器]
---

## Nginx 笔记

### 1. 安装 Homebrew

在 Mac 上使用以下命令安装 Homebrew：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
<!-- more -->
### 2. 安装 Nginx

在终端中运行以下命令来使用 Homebrew 安装 Nginx：

```bash
brew install nginx
```

### 3. 查找 Nginx 配置文件路径

```bash
sudo find / -name "nginx.conf" 2>/dev/null
```

根据您的输出显示，Nginx 配置文件位于 `/opt/homebrew/etc/nginx/nginx.conf`。

### 4. 查看 Nginx 配置文件

```bash
sudo cat /opt/homebrew/etc/nginx/nginx.conf
```

使用上述命令可以查看 Nginx 配置文件的内容，您可以在此文件中进行配置更改。

### 5. 启动、停止和查看 Nginx 状态

启动 Nginx 服务器：

```bash
sudo nginx
```

停止 Nginx 服务器：

```bash
sudo nginx -s stop
```

查看 Nginx 配置是否正确：

```bash
sudo nginx -t
```

查看 Nginx 运行状态：

```bash
sudo nginx -s status
```

### 6. 自动启动 Nginx

如果希望 Nginx 在系统启动时自动运行，可以使用以下命令：

```bash
sudo brew services start nginx
```

### 7. 高级配置

Nginx 具有丰富的功能和选项，您可以根据需求进行高级配置。配置文件路径：`/opt/homebrew/etc/nginx/nginx.conf`

以上是更新后的 Nginx 笔记，现在已经包含了 Homebrew 的安装记录。如果您有其他问题或需要进一步协助，请随时向我询问。

### 8、html静态文件放在哪儿

在使用 Homebrew 安装 Nginx 的情况下，Nginx 的默认根目录路径是 `/opt/homebrew/var/www` 而不是 `/usr/local/var/www`。因此，将 HTML 静态文件放置在正确的 Nginx 根目录下的步骤如下：

1. 将您的 HTML 静态文件复制到 Nginx 根目录：

```bash
cp /path/to/your/html/files/* /opt/homebrew/var/www/
```

请将 `/path/to/your/html/files/` 替换为您实际存放 HTML 静态文件的路径。

2. 在 `/opt/homebrew/etc/nginx/nginx.conf` 配置

```bash
location / {
    root   /opt/homebrew/var/www/sky;
    index  index.html index.htm;
}
````
* `/opt/homebrew/var/www/` 静态文件配置路径
* `sky`是我的项目

3. 确认文件已成功复制到 Nginx 根目录：

```bash
ls /opt/homebrew/var/www/
```

您应该能够看到您复制过来的 HTML 文件列表。

4. 启动或重新加载 Nginx 以使更改生效：

```bash
sudo nginx -s stop  # 先停止 Nginx（如果已经运行）
sudo nginx         # 再启动 Nginx
```

或者，您可以直接使用以下命令来重新加载 Nginx 配置：

```bash
sudo nginx -s reload
```

现在，您的 HTML 静态文件应该位于正确的 Nginx 根目录，可以通过 Nginx 服务器访问了。通过在浏览器中输入 `http://localhost`，您应该能够看到您的静态文件在浏览器中显示。

请注意，如果您在 Nginx 配置中进行了自定义根目录设置，则根目录可能不是默认的 `/opt/homebrew/var/www`，而是您在配置中指定的目录。如果存在自定义根目录，请将 HTML 静态文件放置在自定义根目录下。