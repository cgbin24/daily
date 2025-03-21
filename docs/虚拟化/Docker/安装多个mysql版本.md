理解 Docker 和 MySQL 在 Docker 上的运行机制是非常重要的，特别是当你对虚拟化技术和容器技术不太了解的时候。下面我会详细解释一些核心概念，并且处理使用 Docker 安装 MySQL 的问题。

### 1. **Docker 及其虚拟化原理**

Docker 是一种 **容器化** 技术，它通过容器将应用程序及其所有依赖打包在一起，并在隔离的环境中运行。容器本质上是 **轻量级虚拟机**，它们共享宿主机的操作系统内核，但每个容器都有自己独立的文件系统、网络、进程等。因此，容器和宿主机之间是相互隔离的。

与传统的虚拟机不同，Docker **不需要虚拟化整个操作系统**，而是通过在宿主机上运行多个隔离的进程来实现这一目的。所以，Docker 容器启动和销毁的速度非常快，且资源占用非常少。

### 2. **Docker 容器中的 MySQL 安装**

在 Docker 中运行 MySQL 实际上是在一个容器中运行 MySQL 数据库，而不是直接在宿主机的操作系统上安装。这意味着：

- **容器隔离**：每个容器都是一个独立的环境，容器之间不会直接影响。比如你可以在不同的容器中运行不同版本的 MySQL，它们互不干扰。
- **宿主机不会受影响**：Docker 容器的运行不会改变宿主机的操作系统，只会占用宿主机的一部分资源（如 CPU、内存和磁盘），但是这种占用是非常轻量的，且容器的资源使用可以独立控制。

因此，你可以在 Docker 中运行多个版本的 MySQL，它们之间是相互隔离的，不会影响宿主机的环境，也不会互相干扰。

### 3. **在 Docker 中同时安装多个不同版本的 MySQL**

可以通过以下步骤在 Docker 中同时安装多个不同版本的 MySQL：

- **使用不同的容器**：每个 MySQL 实例都运行在一个独立的容器中，你可以为每个容器指定不同的 MySQL 版本。
- **为每个容器指定不同的端口和数据卷**：每个容器使用不同的端口和挂载的数据卷，这样它们之间不会冲突。

#### 3.1 **如何拉取和运行 MySQL 容器**

你可以通过 Docker 官方提供的 MySQL 镜像来安装 MySQL。不同版本的 MySQL 镜像在 Docker Hub 上都有提供。

例如，使用以下命令来运行 MySQL 5.7：

```bash
docker run --name mysql57 -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 mysql:5.7
```

这条命令会做以下几件事：
- 下载 MySQL 5.7 镜像并启动容器。
- 容器名为 `mysql57`。
- `MYSQL_ROOT_PASSWORD` 设置 MySQL 的 root 用户密码为 `root`。
- 容器的 3306 端口映射到宿主机的 3306 端口。

接下来，如果你想运行 MySQL 8.0 的版本，你可以使用类似的命令，只是镜像的版本变成 `mysql:8.0`：

```bash
docker run --name mysql80 -e MYSQL_ROOT_PASSWORD=root -d -p 3307:3306 mysql:8.0
```

这里的区别是：
- `mysql80` 是容器的名字。
- 3307 端口映射到宿主机的 3307 端口，因为宿主机的 3306 端口已经被 MySQL 5.7 使用了。

#### 3.2 **端口和数据卷**

- **端口**：每个 MySQL 容器都需要一个端口来与宿主机进行通信。如果你运行多个容器，它们需要使用不同的端口。
- **数据卷**：MySQL 容器中的数据是存储在容器的文件系统中的，但通常我们会将数据保存在宿主机上的某个目录，以便数据持久化。你可以通过 `-v` 参数来指定数据卷：

```bash
docker run --name mysql57 -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 -v /my/own/datadir:/var/lib/mysql mysql:5.7
```

这个命令会将容器中的 `/var/lib/mysql` 目录挂载到宿主机的 `/my/own/datadir` 目录，这样即使容器被销毁，数据仍然保存在宿主机上。

### 4. **MySQL 数据库的管理**

使用 Docker 容器运行 MySQL 后，你可以使用标准的 MySQL 客户端（如 `mysql` 命令行工具或 GUI 工具如 DBeaver、Navicat 等）连接到容器内的 MySQL 实例。

连接 MySQL 5.7 容器：

```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
```

连接 MySQL 8.0 容器：

```bash
mysql -h 127.0.0.1 -P 3307 -u root -p
```

### 5. **总结**

1. **Docker 是容器化技术**：Docker 提供了一个隔离的环境来运行应用程序，像 MySQL 这样的服务可以在容器中运行而不影响宿主机。
2. **多个 MySQL 版本**：你可以在 Docker 中同时运行多个不同版本的 MySQL，只要它们使用不同的端口和数据卷即可互不干扰。
3. **宿主机隔离**：Docker 容器与宿主机是隔离的，容器中的 MySQL 与宿主机的其他应用程序没有直接关系，不会对宿主机造成影响。
4. **容器管理**：通过 Docker 命令，你可以轻松管理容器，包括启动、停止、删除和查看容器的状态。

通过这些步骤，你可以在 Mac 上通过 Docker 安全地运行多个 MySQL 实例，且不会对宿主机环境造成影响。

{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://cr.console.aliyun.com",
    "http://hub-mirror.c.163.com",
    "https://f1361db2.m.daocloud.io"
  ]
}