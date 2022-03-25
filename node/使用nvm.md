#### `windows` 使用 `nvm` （涉及nvm && node && npm 安装、卸载）

> [关于 `nvm` 相关概述请参阅](https://github.com/coreybutler/nvm-windows)

##### 1、下载

[`nvm 下载`](https://github.com/coreybutler/nvm-windows/releases)

> 找到 `Assets` -----> `nvm-setup.zip`

##### 2、安装

- 下载完成后，解压文件，并找到 `nvm-setup.exe` 双击打开
- 指定 `nvm` 安装路径（系统会自动将该路径添加到系统环境变量中）
- 指定符号链（symlink --> nodejs）配置路径（系统会自动将该路径添加到系统环境变量中）
  - 这里个人推荐指定路径与 `nvm` 路径同一级

##### *****3、使用 *`管理员身份`* 启动控制台查看安装结果

> **重要**：这里必须使用 `管理员身份`， 否则可能出现安装失败问题，如：可能会遇到 `node` 安装成功，但没有 `npm` 的情况

```shell
nvm
或
nvm version // 查看版本

// 显示如下信息则表示成功

Running version 1.1.9.

Usage:

  nvm arch                     : Show if node is running in 32 or 64 bit mode.
  nvm current                  : Display active version.
  nvm install <version> [arch] : The version can be a specific version, "latest" for the latest current version, or "lts" for the ......
或
1.1.9

```

##### 4、安装指定版本的 `node` 环境

> 在此之前，还需要先配置镜像源（这会使得后续下载安装的速度得到明显提升）

###### 配置镜像流程

- 切换到 `nvm` 安装目录，找到 `setting.txt` 文件并打开

- 将下面镜像信息添加进去后保存退出即可，当然这里使用的是 **淘宝镜像**，也可根据个人需求配置其他镜像

  ```shell
  node_mirror: https://npm.taobao.org/mirrors/node/
  npm_mirror: https://npm.taobao.org/mirrors/npm/
  ```

- 完整文件信息如下：

  ```shell
  root: D:\Software\nvm
  path: D:\Software\nodejs
  node_mirror: https://npm.taobao.org/mirrors/node/
  npm_mirror: https://npm.taobao.org/mirrors/npm/
  ```

###### 安装 `node` 环境

```shell
nvm install [node version]
如：
nvm install 14.14.0

// 显示如下信息则表示成功
Downloading node.js version 14.14.0 (64-bit)...
Complete
Downloading npm version 6.14.8... Complete
Installing npm v6.14.8...

Installation complete. If you want to use this version, type

nvm use 14.14.0
```

##### 5、查看 `node` 安装结果

```shell
nvm list

// 显示以下信息则表示成功

    14.14.0

```

##### 6、切换使用 `node` 版本环境（默认未激活）

```shell
nvm use [node version]
如：
nvm use 14.14.0

// 显示以下信息则表示成功
Now using node v14.14.0 (64-bit)

```

##### 7、查看 `node` 安装结果

```shell
node // 进入node环境则成功
或
node -v // 显示对应版本则成功

// 显示以下信息则表示成功
Welcome to Node.js v14.14.0.
Type ".help" for more information.
> 
或
v14.14.0

```

##### 8、查看 `npm` 安装结果

```shell
npm
或
npm -v

// 显示以下信息则表示成功

Usage: npm <command>

where <command> is one of:
    access, adduser, audit, bin, bugs, c, cache, ci, cit,
    clean-install, clean-install-test, completion, config,
    create, ddp, dedupe, deprecate, dist-tag, docs, doctor,
    edit, explore, fund, get, help, help-search, hook, i, init,
    install, install-ci-test, install-test, it, link, list, ln,
    login, logout, ls, org, outdated, owner, pack, ping, prefix,
    profile, prune, publish, rb, rebuild, repo, restart, root,
    run, run-script, s, se, search, set, shrinkwrap, star,
    stars, start, stop, t, team, test, token, tst, un,
    uninstall, unpublish, unstar, up, update, v, version, view,
    whoami

npm <command> -h  quick help on <command>
npm -l            display full usage info
npm help <term>   search for help on <term>
npm help npm      involved overview

Specify configs in the ini-formatted file:
    C:\Users\Administrator\.npmrc
or on the command line via: npm <command> --key value
Config info can be viewed via: npm help config

或
6.14.8

```

> 提示：[查看 `node.js` 版本和 `npm` 版本对应信息](https://nodejs.org/zh-cn/download/releases/)



**至此，简单的安装流程就告一段落了 *★,°*:.☆(￣▽￣)/$:*.°★* 。**



##### 关于卸载:

###### `node` 卸载

> 当前 `node` 托管于 `nvm`, 因此可通过 `nvm` 命令尽心卸载

```shell
nvm uninstall [node version]
如:
nvm uninstall 14.14.0

// 显示以下信息则表示成功
Uninstalling node v14.14.0... done
```

###### `nvm` 卸载

> 打开系统设置 ----> 找到应用管理页 ----> 找到 `NVM for Windows` ----> 卸载即可

###### `nvm` 常用指令

|           命令           |                             说明                             |
| :----------------------: | :----------------------------------------------------------: |
|         nvm list         |                      查看已经安装的版本                      |
|    nvm list installed    |                      查看已经安装的版本                      |
|    nvm list available    |                    查看网络可以安装的版本                    |
|         nvm arch         |             查看当前系统的位数和当前nodejs的位数             |
|    nvm install [arch]    | 安装制定版本的node 并且可以指定平台 version 版本号 arch 平台 |
|          nvm on          |                      打开nodejs版本控制                      |
|         nvm off          |                      关闭nodejs版本控制                      |
|     nvm proxy [url]      |                        查看和设置代理                        |
|  nvm node_mirror [url]   | 设置或者查看setting.txt中的node_mirror，若不设置则默认是 https://nodejs.org/dist/ |
|   nvm npm_mirror [url]   | 设置或者查看setting.txt中的npm_mirror, 若不设置则默认是：https://github.com/npm/npm/archive/ |
|      nvm uninstall       |                        卸载指定的版本                        |
| nvm use [version] [arch] |                   切换指定的node版本和位数                   |
|     nvm root [path]      |                      设置和查看root路径                      |
|       nvm version        |                        查看当前的版本                        |



**备注:  鉴于不同系统环境和相关版本差异, 若存在未涉及或表述不恰当内容可留言探讨**

