# 从 github 或 gitee 上获取数据

### github 获取 json

### 1、 `raw.githubusercontent.com`

> 使用 `raw.githubusercontent.com`，去掉 `blob，github提供的读取资源文件格式如下：

```bash
https://raw.githubusercontent.com/${owner}/${repo}/${Branch}/${path}
```

**事例如下：**

> github 仓库文件路径

```
https://github.com/username/project/blob/master/test.json
```

> 获取原始数据路径

```
https://raw.githubusercontent.com/username/project/master/test.json
```

基于国内的访问速度不是很理想，那便可参考如下方式

### 2、 `api.github.com`

> 第二种方式，请求接口时，需要添加额外的 `headers` 信息，而且有次数限制，未验证的客户端每小时只能请求60次。

```bash
https://api.github.com/repos/${username}/${repo}/contents/${apth}?ref=${branch}

headers: {
  Accept: 'application/vnd.github.v3.raw',
}
```

这种方式在国内的访问速度比第一种好。

### 3、other

> 使用 github 的 github pages 功能，将 json 文件部署到上面，在国内的访问速度也还可以，具体部署方式请参考[官方文档](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)。



### gitee 获取 json 方式

> gitee 获取 json 的方式更简单一些，只需要将 gitee 仓库路径的 `blob` 修改为 `raw` 即可。

**事例如下：**

> 原始仓库路径

```
https://gitee.com/username/project/blob/master/data.json
```

> 获取原始数据的路径

```
https://gitee.com/username/project/raw/master/data.json
```

最后，国内用户还是推荐使用 gitee