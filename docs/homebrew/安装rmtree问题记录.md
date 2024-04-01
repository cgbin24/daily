### 问题

> 使用 `homebrew` 安装应用时，经常遇到所安装的应用会携带自身的依赖项，但是在使用 `uninstall` 卸载时却不能将依赖一起卸载，很是苦恼。

然后 **“有需求，就会有解决方案”**

有这样一个工具，它可以在卸载前搜集卸载对象相关的依赖并列出确认，然后再将所以内容一起卸载干净，这就是 [homebrew-rmtree](https://github.com/beeftornado/homebrew-rmtree)，安装/使用方式可访问连接学习

然鹅，安装过程中出现了点小问题
```sh
➜  ~ brew tap beeftornado/rmtree
==> Tapping beeftornado/rmtree
Cloning into '/opt/homebrew/Library/Taps/beeftornado/homebrew-rmtree'...
fatal: unable to access 'https://github.com/beeftornado/homebrew-rmtree/': Failed to connect to github.com port 443 after 75026 ms: Couldn't connect to server
Error: Failure while executing; `git clone https://github.com/beeftornado/homebrew-rmtree /opt/homebrew/Library/Taps/beeftornado/homebrew-rmtree --origin=origin --template= --config core.fsmonitor=false` exited with 128.
➜  ~
```

### 解决方案

```sh
➜  ~ rm -rf "/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core"
➜  ~
```

```sh
➜  ~ brew tap homebrew/core
HOMEBREW_CORE_GIT_REMOTE set: using https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git as the Homebrew/homebrew-core Git remote.
➜  ~
```


#### 参考

[Error: Failure while executing; git clone https://github.com/Homebrew/homebrew-cask /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask --origin=origin --template= exited with 128](https://www.oomake.com/question/16463736)

[homebrew-rmtree](https://github.com/beeftornado/homebrew-rmtree/)
