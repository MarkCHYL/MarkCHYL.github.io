---
layout: post
title: ubuntu中安装mysql
toc: true
reward: true
date: 2023-09-08 17:12:03
tags: [mysql]
categories: [ubuntu]
---
要在Ubuntu中安装MySQL，可以按照以下步骤进行操作：

1. 打开终端：你可以按下`Ctrl+Alt+T`组合键来打开终端。

2. 更新包列表：首先，运行以下命令来确保你的系统上的包列表是最新的：

   ```
   sudo apt update
   ```
<!-- more -->
3. 安装MySQL服务器：运行以下命令来安装MySQL服务器：

   ```
   sudo apt install mysql-server
   ```

   在安装过程中，系统会要求你设置MySQL root用户的密码。请务必选择一个强密码，并确保记住它。

4. 启动MySQL服务：安装完成后，MySQL服务将会自动启动。你可以使用以下命令来检查MySQL服务状态：

   ```
   sudo systemctl status mysql
   ```

   如果MySQL没有自动启动，你可以使用以下命令手动启动它：

   ```
   sudo systemctl start mysql
   ```

5. 配置MySQL安全性：运行以下命令来配置MySQL的基本安全性：

   ```
   sudo mysql_secure_installation
   ```

   在这个过程中，你可以选择进行一些基本的安全设置，如删除匿名用户、禁止远程root登录等。按照提示进行操作即可。

6. 连接到MySQL：安装完成后，你可以使用以下命令来连接到MySQL服务器：

   ```
   mysql -u root -p
   ```

   然后输入你之前设置的MySQL root密码。

现在，你已经成功在Ubuntu上安装了MySQL数据库服务器。你可以使用命令行工具或图形界面工具（如phpMyAdmin）来管理MySQL数据库。如果需要更多详细信息或有其他问题，请随时提问。

### 安装过程中没有要求我设置账号密码，我现在需要重新安装
如果在安装MySQL过程中没有要求你设置账号密码，你可以尝试重新安装MySQL并确保按照以下步骤操作：

1. 首先，确保你的系统处于最新状态，以获取最新的软件包信息：

   ```
   sudo apt update
   ```

2. 然后，安装MySQL服务器：

   ```
   sudo apt install mysql-server
   ```

3. 安装过程中，你应该会看到一个对话框，要求你设置MySQL root用户的密码。如果你没有看到这个对话框，可以尝试卸载MySQL并重新安装：

   ```
   sudo apt remove --purge mysql-server mysql-client mysql-common
   sudo apt autoremove
   sudo apt autoclean
   ```

4. 完成卸载后，再次安装MySQL服务器：

   ```
   sudo apt install mysql-server
   ```

5. 在重新安装MySQL时，确保注意看控制台上的任何提示，以设置MySQL root 用户的密码。遵循提示进行操作。

6. 安装完成后，你可以使用以下命令来检查MySQL服务状态：

   ```
   sudo systemctl status mysql
   ```
### sudo apt install mysql-server执行后没有提示设置密码。默认密码是啥

如果在安装MySQL时没有提示设置密码，并且没有默认密码，那么可能是因为MySQL版本或配置文件的不同。在某些MySQL安装中，默认情况下可能没有设置root密码。

尝试使用以下步骤来设置root密码：

1. 通过以下方式登录到MySQL服务器：

   ```
   sudo mysql
   ```

2. 在MySQL中，使用以下命令来更改root用户的密码（将`your_new_password`替换为你希望设置的新密码）：

   ```
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';
   ```

3. 刷新MySQL权限：

   ```
   FLUSH PRIVILEGES;
   ```

4. 退出MySQL：

   ```
   EXIT;
   ```

现在，你应该已经成功设置了MySQL root 用户的密码。你可以使用新密码来登录MySQL。

### ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
这个错误是因为MySQL的密码策略要求密码必须符合一定的安全性要求。默认情况下，MySQL要求密码满足以下条件：

1. 至少包含8个字符。
2. 包含大写字母。
3. 包含小写字母。
4. 包含数字。
5. 包含特殊字符（例如：!、@、#、$等）。

如果你的密码不符合这些要求，你可以尝试以下方法来解决这个问题：

1. **使用更强密码**：尝试创建一个更复杂的密码，满足上述要求。

2. **修改密码策略**：如果你不想使用这些复杂的密码策略，可以修改MySQL的密码策略。但要注意，这可能会降低数据库的安全性。要修改密码策略，可以按照以下步骤：

   - 编辑MySQL配置文件（通常是`/etc/mysql/mysql.conf.d/mysqld.cnf`）：

     ```
     sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
     ```

   - 在配置文件中找到或添加以下行来禁用密码策略：

     ```
     skip-policy
     ```

   - 保存文件并退出编辑器。

   - 重新启动MySQL服务：

     ```
     sudo systemctl restart mysql
     ```

3. **使用密码生成工具**：你还可以使用密码生成工具来创建一个符合要求的密码，然后将其用作MySQL的密码。例如：

   ```
   openssl rand -base64 12
   ```

   这将生成一个12个字符长的随机密码。

无论你选择哪种方法，都需要确保你的MySQL密码足够安全，以保护数据库的安全性。如果你对密码策略和安全性有疑问，最好咨询数据库管理员或安全专家的建议。