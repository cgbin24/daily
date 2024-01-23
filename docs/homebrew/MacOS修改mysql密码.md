> 修改mysql密码


问题
```sh
Your password does not satisfy the current policy requirements'
```

```sh
umac@umacdexuniji Dev % 
umac@umacdexuniji Dev % $ sudo ufw allow 3306

zsh: command not found: $
umac@umacdexuniji Dev % sudo ufw allow 3306 

Password:
sudo: ufw: command not found
umac@umacdexuniji Dev % systemctl status mysql
zsh: command not found: systemctl
umac@umacdexuniji Dev % telnet 127.0.0.1 3306
zsh: command not found: telnet
umac@umacdexuniji Dev % /Applications/                    
zsh: permission denied: /Applications/
umac@umacdexuniji Dev % ls /Applications 
Apifox.app              Microsoft Edge.app      Utilities
BaiduNetdisk_mac.app    Navicat Premium.app     Visual Studio Code.app
DBeaver.app             Safari.app              iTerm.app
JetBrains Toolbox.app   Tencent Lemon.app
umac@umacdexuniji Dev % mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.2.0 Homebrew

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
mysql> 
mysql> 
mysql> alter user 'root'@'localhost' identified with mysql_nativ_password by 'c_m@sql7';
ERROR 1524 (HY000): Plugin 'mysql_nativ_password' is not loaded
mysql> alter user 'root'@'localhost' identified with mysql_native_password by 'c_m@sql7';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '123456';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> alter user 'root'@'localhost' identified with mysql_native_password by 'cgbin123456';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> alert user 'root'@'localhost' identified by 'cgb123321';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'alert user 'root'@'localhost' identified by 'cgb123321'' at line 1
mysql> mysqladmin -u username -h hostname -p password 'cgbin123123'
    -> ;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'mysqladmin -u username -h hostname -p password 'cgbin123123'' at line 1
mysql> mysqladmin -u root -h hostname -p password 'cgbin123123'
    -> ;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'mysqladmin -u root -h hostname -p password 'cgbin123123'' at line 1
mysql> mysqladmin -u root -h localhost -p 'c_m@sql7' 'cgbin123123'
    -> ;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'mysqladmin -u root -h localhost -p 'c_m@sql7' 'cgbin123123'' at line 1
mysql> exit
Bye
umac@umacdexuniji Dev % mysqladmin -u root -h -p password 'umac123'     
mysqladmin: connect to server at '-p' failed
error: 'Unknown MySQL server host '-p' (8)'
Check that mysqld is running on -p and that the port is 3306.
You can check this by doing 'telnet -p 3306'
umac@umacdexuniji Dev % mysqladmin -u root  -p password 'umac123' 
Enter password: 
mysqladmin: [Warning] Using a password on the command line interface can be insecure.
Warning: Since password will be sent to server in plain text, use ssl connection to ensure password safety.
mysqladmin: unable to change password; error: 'Your password does not satisfy the current policy requirements'
```

```sh
umac@umacdexuniji Dev % mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.2.0 Homebrew

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select @@validate_password_policy;
ERROR 1193 (HY000): Unknown system variable 'validate_password_policy'
mysql> show variables like 'validate_password%';
+-------------------------------------------------+--------+
| Variable_name                                   | Value  |
+-------------------------------------------------+--------+
| validate_password.changed_characters_percentage | 0      |
| validate_password.check_user_name               | ON     |
| validate_password.dictionary_file               |        |
| validate_password.length                        | 8      |
| validate_password.mixed_case_count              | 1      |
| validate_password.number_count                  | 1      |
| validate_password.policy                        | MEDIUM |
| validate_password.special_char_count            | 1      |
+-------------------------------------------------+--------+
8 rows in set (0.00 sec)

mysql> set global validate_password_policy=0;
ERROR 1193 (HY000): Unknown system variable 'validate_password_policy'
mysql> set global validate_password.policy=0;
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password.length=1;
Query OK, 0 rows affected (0.00 sec)

mysql> alter user 'root'@'localhost' identified by 'umac123';
Query OK, 0 rows affected (0.01 sec)

mysql> 
```