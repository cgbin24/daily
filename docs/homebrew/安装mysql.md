#### HomeBrew安装mysql

##### Shell指令：

```bash
brew install mysql	#安装

mysql --version	#查看mysql版本

brew services start mysql	#启动mysql

mysql_secure_installation
#设置安全运行(因root默认没密码)、配置信息如密码(具体可参照衍生阅读文献)

mysql -uroot -p	#连接mysql

> exit	#退出mysql
```

##### 踩坑:

> **注意:** 成功安装后需先启动 `mysql` , 即先执行 `brew services start mysql` , 若直接执行 `mysql_secure_installation` 则可能会出现以下错误

```bash
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/temp/mysql.sock' (2)
```

##### 使用方式：

> 安装完成后可使用以下方式登录查看

```bash
mysql -uroot -p	#root用户、使用密码登录

> exit #退出mysql环境
```

##### > 衍生阅读:

[Mac系统homebrew安装MySQL等环境](https://blog.csdn.net/u013488276/article/details/124981958)

[Homebrew安装mysql](https://www.jianshu.com/p/d8e89be81906)