Docker 是一个强大的容器化平台，能够让你轻松地创建、部署、管理和扩展应用程序。Docker 通过容器技术简化了开发、测试、部署和运维工作。以下是一些常用的 Docker 命令、技巧和管理方式，可以帮助你更高效地使用 Docker。

### 1. **Docker 基础命令**

#### 1.1 **查看 Docker 版本**
查看 Docker 的版本信息，包括客户端和服务端的版本：

```bash
docker --version
```

#### 1.2 **查看系统信息**
显示 Docker 的系统信息，包括容器、镜像、存储驱动、资源限制等：

```bash
docker info
```

### 2. **容器管理**

#### 2.1 **列出所有容器**
列出所有容器，包括正在运行的和已停止的：

```bash
docker ps -a
```

要只列出正在运行的容器：

```bash
docker ps
```

#### 2.2 **启动容器**
启动一个已停止的容器：

```bash
docker start <容器ID或容器名>
```

#### 2.3 **停止容器**
停止正在运行的容器：

```bash
docker stop <容器ID或容器名>
```

#### 2.4 **强制停止容器**
如果容器无法正常停止，可以强制停止：

```bash
docker kill <容器ID或容器名>
```

#### 2.5 **删除容器**
删除一个已停止的容器：

```bash
docker rm <容器ID或容器名>
```

删除所有已停止的容器：

```bash
docker rm $(docker ps -a -q)
```

#### 2.6 **查看容器的日志**
查看容器的标准输出和标准错误日志：

```bash
docker logs <容器ID或容器名>
```

查看实时日志（与 `tail -f` 类似）：

```bash
docker logs -f <容器ID或容器名>
```

#### 2.7 **进入容器**
如果需要进入容器并在其中执行命令，可以使用 `docker exec` 命令：

```bash
docker exec -it <容器ID或容器名> /bin/bash
```

进入容器后，你将拥有一个交互式的终端，可以在容器内部执行命令。

#### 2.8 **容器内进程监控**
查看容器内运行的进程：

```bash
docker top <容器ID或容器名>
```

### 3. **镜像管理**

#### 3.1 **列出本地镜像**
查看本地存储的所有镜像：

```bash
docker images
```

#### 3.2 **拉取镜像**
从 Docker Hub 或私有仓库拉取镜像：

```bash
docker pull <镜像名>:<标签>
```

例如，拉取最新的 MySQL 镜像：

```bash
docker pull mysql:latest
```

#### 3.3 **构建镜像**
根据 `Dockerfile` 创建镜像：

```bash
docker build -t <镜像名>:<标签> <路径>
```

例如，使用当前目录下的 Dockerfile 构建镜像：

```bash
docker build -t myapp:latest .
```

#### 3.4 **删除镜像**
删除一个镜像：

```bash
docker rmi <镜像ID或镜像名>
```

删除未使用的镜像：

```bash
docker image prune
```

#### 3.5 **强制删除镜像**
如果镜像仍被容器使用，可以强制删除：

```bash
docker rmi -f <镜像ID或镜像名>
```

#### 3.6 **推送镜像**
将镜像推送到 Docker Hub 或其他容器仓库：

```bash
docker push <镜像名>:<标签>
```

### 4. **网络管理**

#### 4.1 **查看网络**
列出所有网络：

```bash
docker network ls
```

#### 4.2 **创建网络**
创建一个自定义的网络：

```bash
docker network create <网络名>
```

#### 4.3 **查看网络详细信息**
查看某个网络的详细信息：

```bash
docker network inspect <网络名>
```

#### 4.4 **连接容器到网络**
将容器连接到一个指定的网络：

```bash
docker network connect <网络名> <容器ID或容器名>
```

#### 4.5 **断开容器与网络的连接**
将容器从网络中断开：

```bash
docker network disconnect <网络名> <容器ID或容器名>
```

### 5. **数据管理**

#### 5.1 **查看卷**
查看所有 Docker 卷：

```bash
docker volume ls
```

#### 5.2 **创建卷**
创建一个卷：

```bash
docker volume create <卷名>
```

#### 5.3 **查看卷详细信息**
查看某个卷的详细信息：

```bash
docker volume inspect <卷名>
```

#### 5.4 **删除卷**
删除卷：

```bash
docker volume rm <卷名>
```

#### 5.5 **挂载卷到容器**
在运行容器时挂载卷：

```bash
docker run -v <宿主机目录>:<容器目录> <镜像名>
```

例如，将宿主机 `/my/data` 目录挂载到容器内的 `/data` 目录：

```bash
docker run -v /my/data:/data myimage
```

### 6. **Docker Compose**

Docker Compose 是一个工具，用于定义和运行多容器的 Docker 应用。它通过一个 YAML 文件来配置应用的服务、网络、卷等。

#### 6.1 **启动 Compose 应用**
通过 Compose 文件启动应用：

```bash
docker-compose up
```

如果想在后台运行：

```bash
docker-compose up -d
```

#### 6.2 **停止 Compose 应用**
停止 Compose 应用：

```bash
docker-compose down
```

#### 6.3 **查看 Compose 服务状态**
查看 Compose 应用中各服务的状态：

```bash
docker-compose ps
```

#### 6.4 **构建 Compose 应用**
如果你修改了 `docker-compose.yml` 文件，可以通过以下命令重新构建镜像：

```bash
docker-compose build
```

### 7. **常见技巧与清理**

#### 7.1 **清理未使用的资源**
你可以使用 `docker system prune` 命令来清理无用的资源（停止的容器、未使用的镜像、未使用的网络等）：

```bash
docker system prune
```

加上 `-a` 参数来清理所有未使用的镜像：

```bash
docker system prune -a
```

#### 7.2 **查看容器的资源使用情况**
你可以使用 `docker stats` 来查看当前运行容器的资源使用情况（包括 CPU、内存、磁盘 I/O 等）：

```bash
docker stats
```

#### 7.3 **通过标签标记镜像**
你可以为镜像打标签，帮助管理不同版本的镜像：

```bash
docker tag <镜像ID> <镜像名>:<标签>
```

### 8. **Docker 容器优化**

#### 8.1 **使用 `.dockerignore` 文件**
在构建镜像时，Docker 会将当前目录中的所有文件添加到镜像中。使用 `.dockerignore` 文件来忽略不需要的文件或目录，以减小镜像的大小。

#### 8.2 **优化镜像构建**
将构建过程中较少变化的步骤放在 Dockerfile 的前面，能更好地利用 Docker 的缓存机制，从而减少构建时间。

---

### 总结

这些 Docker 命令和技巧覆盖了 Docker 日常使用中的大部分操作，包括容器和镜像管理、网络和数据管理、Docker Compose 使用等。通过这些命令，你可以更高效地进行开发、部署和运维工作。