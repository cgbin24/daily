### 问题

> 本机与`Github`均已配置`SSH`密钥，不明所以的在使用时突然失效了，无法完成代码拉取与推送

```sh
Cloning into 'example-dev'...
no such identity: /Users/example/.ssh/id_rsa # �\205��\222��\232\204�\230�\224��\215置: No such file or directory
git@ssh.github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

### 实践

- [测试 `SSH` 连接](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)

```sh
➜  ~ ssh -T git@github.com
no such identity: /Users/example/.ssh/id_rsa # �\205��\222��\232\204�\230�\224��\215置: No such file or directory
git@ssh.github.com: Permission denied (publickey).
```

- [生成新的 `SSH` 密钥](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
  
  > 快速解决战斗

```sh
➜  ~ ssh-keygen -t rsa -b 4096 -C "example@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/example/.ssh/id_rsa):
/Users/example/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/example/.ssh/id_rsa
Your public key has been saved in /Users/example/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:0MR5E5Xu5eI/2q3NPhGv0MBleO1vnXwNupRtMhFXuI8 example@example.com
The key's randomart image is:
+---[RSA 4096]----+
|       ....o.o +.|
|       oo o + * .|
|      . .. + * o |
|       .    = +..|
|        S  . X.+B|
|            X E+O|
|           o B oo|
|            o.o= |
|            .o++*|
+----[SHA256]-----+
```

- 查看host

```sh
➜  ~ cat ~/.ssh/config
Host github.com
User "example@example.com #注册github的邮箱"
HostName ssh.github.com
PreferredAuthentications publickey
IdentityFile "~/.ssh/id_rsa # 公钥的存放位置"
Port 443%
```

- 列出 `.ssh` 下的所有文件

```sh
➜  ~ ls -al ~/.ssh
total 80
drwxr-xr-x  10 example  staff   320  3 22 09:55 .
drwxr-x---+ 53 example  staff  1696  3 22 09:57 ..
-rw-r--r--@  1 example  staff  6148  3 22 09:55 .DS_Store
drwxr-xr-x   3 example  staff    96  7 30  2023 code_gitpod.d
-rw-r--r--@  1 example  staff   181  3 22 09:55 config
-rw-------   1 example  staff   157  1 18 21:43 config.save
-rw-------@  1 example  staff  3434  3 22 09:56 id_rsa
-rw-rw-r--@  1 example  staff   740  3 22 09:56 id_rsa.pub
-rw-------   1 example  staff  5105  1 18 21:49 known_hosts
-rw-------   1 example  staff  5003  9 21  2023 known_hosts.old
```

- 查看并复制公钥、添加到`Github`

```sh
➜  ~ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCua/PsTGLMRSzjrQ7elsdjfJINNmsi239HDsIsdf238823nkansdat0yCljQ+Jcg/QbDyYcEyVKwUYj22pEhDLtaVA1pF2s4Oc0+H1Hhbg0SUzX8q6v+jYlJ80xFipxlnwvclkoFXrXhIdeykjqVPCD+nyTIN8ymqTHhoGZxYPvAoXpNjZfU9lOM4MHX5e5ryZlpwhb5IWbLjxSeNgGkxenxURtpSY9Aiy6Ik8PIwb1XWNd4910S4kTMu4+6ia/OYjM+23u42234234jlajsdlfjalsdjf329u9234klajsdlfjlj32asdfa/cCuOmjhLG8iBNX3uvgHFAZhfsrmOBKw5CoMH7pNNXaI/RU5qs4MaJZHpc9AXn1H2PrD9KlYj9aY5gBqJkFvDzCFMc868l5N9DXBrkO8HWhRdWcoSw62SB+mgNcMX7idySaCtn9UAsrA2xyZt1ZiiQMd48tNmNzNz5Pj0Lb43ptoi6jL19CHXHeDdB6ZUMjg1qvwk0QyOPZqGaZs96bm9DQm/YSeraQqLZSjPz4pMKc7AOaXle9ZITnQdfG+TSnubUpV+iEvPH0I5YjTzEXjmW2+DJDLJssdfJDIjfajsdfei23890sdjo2s8djs8jeosu38sD== example@example.com
➜  ~
➜  ~
```

- 测试本地密钥是否正常工作

```sh
➜  ~ ssh -T git@github.com
no such identity: /Users/example/.ssh/id_rsa # �\205��\222��\232\204�\230�\224��\215置: No such file or directory
git@ssh.github.com: Permission denied (publickey).
```

- [添加代理](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)

> 通过代理可以简化操作，它会将密钥加载到内存中，不需要每次使用密钥时都输入密码

```sh
➜  ~ ssh-agent -s
SSH_AUTH_SOCK=/var/folders/56/cw831_v949x7s__p4vxnf7_r0000gn/T//ssh-QcQgcbbvvYi7/agent.86654; export SSH_AUTH_SOCK;
SSH_AGENT_PID=86655; export SSH_AGENT_PID;
echo Agent pid 86655;
➜  ~
```

- 将 `SSH` 密钥添加到 `ssh-agent`

```sh
➜  ~ ssh-add ~/.ssh/id_rsa
Enter passphrase for /Users/example/.ssh/id_rsa:
Identity added: /Users/example/.ssh/id_rsa (example@example.com)
➜  ~
➜  ~
```

- 再次测试

```sh
➜  ~ ssh -T git@github.com
Hi example! You've successfully authenticated, but GitHub does not provide shell access.
➜  ~
➜  ~
```

### 效果

> 成功拉取代码

```sh
Cloning into 'exmaple-dev'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```