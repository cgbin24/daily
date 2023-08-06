### Nginx使用多个.conf文件配置虚拟主机server
> 使用 `Nginx` 配置多个虚拟机 `server` 服务。通常做法可以直接在 `nginx.conf` 文件中添加即可，如下事例：
```nginx
# nginx.conf

worker_processes  1;

events {
  worker_connections  1024;
}

http {
  include         mime.types;
  default_type    application/octet-stream;

  server {
    listen        80;
    server_name   localhost;
    location  / {
      root        html;
      index       index.html index.htm;
    }
  }

  server {
    listen        3000;
    server_name   www.example.com;
    location  / {
      root        /example/dist;
      index       index.html index.htm;
    }
  }
  ...
}
```

这样写其实没问题，只不过不易扩展，或者说 `server` 服务多了之后管理起来不是很方便，通常，我更愿意将一个 `server` 服务模块单独抽离出来进行管理，而在 `nginx.conf` 中呢只需要将各个独立配置的模块导入即可，既增加了其扩展性，同时每一个单独模块配置便与主配置文件分离，即使其内部配置出错了也不会影响到主配置文件，那么如何实现呢，请往下看

- 第一步
> 在 `conf` 目录下创建一个 `servers` 目录用于管理所有的 `server` 模块
```
| -- conf
     | -- servers
```

- 第二步
> 在 `servers` 目录下创建以 `.conf` 结尾的 `server` 配置文件，如
```
| -- conf
     | -- servers
          example.conf
```

- 第三步
> 将 `server` 具体配置进行完善，这里只需要 `server` 块的内容即可，且注意每一行结尾以 `;` 表示结束，不然会报错，事例如下
```nginx
# servers/example.conf

server {
  listen        3000;             # 端口号
  server_name   www.example.com;  # 服务名 - 访问的域名
  location  / {
    root        /example/dist;
    index       index.html index.htm;
  }
}
```

- 第四步
> 到这里，漫漫长征路已经接近尾声，该创建的文件目录都有了，该有的配置也完善了，最后就需要将所有的配置导入（包含）到 `nginx.conf` 中来，那么回到 `nginx.conf` 中找到 `http` 模块，并在内部添加如下内容
```nginx
include servers/*.conf
```
即
> 其中的 `...` 代表内容省略，这条配置的意思就是 `包含servers目录下的所有以 .conf 结尾的文件`，即此后所有在此目录下创建的 `.conf` 文件都会被导入（包含）到 `nginx.conf` 配置当中，若想创建多个 `server` 服务，重复【第三步】操作即可
```nginx
# nginx.conf

...
http {
  ...
  include servers/*.conf
  ...
}
...
```

如此，`nginx.conf` 文件可简化为
```nginx
# nginx.conf

worker_processes  1;

events {
  worker_connections  1024;
}

http {
  include         mime.types;
  default_type    application/octet-stream;
  include servers/*.conf

  server {
    listen        80;
    server_name   localhost;
    location  / {
      root        html;
      index       index.html index.htm;
    }
  }
  ...
}
```
=== OR ===
> 下面也可将 `80` 端口的 `server` 服务也单独拆分出来
```nginx
# servers/local.conf

server {
  listen        80;
  server_name   localhost;
  location  / {
    root        html;
    index       index.html index.htm;
  }
}
```

```nginx
# nginx.conf

worker_processes  1;

events {
  worker_connections  1024;
}

http {
  include         mime.types;
  default_type    application/octet-stream;
  include servers/*.conf

  ...
}
```

### :: 重要
> 每次修改完配置之后都要记得 `reload` 一下，不然是不会生效的，执行以下命令，控制台不报错且没有任何内容输出即为成功，若有报错请按提示操作即可，一般情况问题主要出现在配置内容上，请仔细检查
```shell
nginx -s reload
```

#### :: 衍伸阅读
[nginx documentation](https://nginx.org/en/docs/)

## :: 更多内容 ::
[> 开发过程中踩坑经验记录](https://github.com/cgbin24/daily)