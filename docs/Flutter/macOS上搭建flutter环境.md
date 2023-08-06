#### macOS上搭建flutter环境

#### 安装
> 下载完成后，将文件解压到指定文件夹位置（相当于安装路径）
[下载flutter](https://docs.flutter.dev/get-started/install/macos)

#### 环境配置
> 配置环境变量到全局
【配置文件一】
- 步骤 1
```shell
vim ~/.bash_profile
```
- 步骤 2
```shell
export PATH=[flutter安装的bin路径]:$PATH
如：
export PATH=/Users/martin/flutter_sdk/flutter/bin:$PATH

export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```
- 步骤 3
> 刷新文件，让配置环境生效
```shell
source ~/.bash_profile
```

【配置文件二】
- 步骤 1
```shell
vim ~/.zshrc
```
- 步骤 2
```shell
export PATH=[flutter安装的bin路径]:$PATH
如：
export PATH=/Users/martin/flutter_sdk/flutter/bin:$PATH

export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```
- 步骤 3
> 刷新文件，让配置环境生效
```shell
source ~/.zshrc
```

#### 验证
> 控制台任意路径下尝试输入以下命令查看，成功看到 `flutter` 版本输入即完成
```shell
flutter --version
```
下一步输入以下指令检测环境，确认是否生效
```shell
flutter doctor
```

#### `Xcode 下载`
> 如果你也需要在 `虚拟机Mac` 中安装 `Xcode` ，可以使用如下地址在官网下载，不过需要使用 `apple ID` 登录
[Xcode_14](https://developer.apple.com/services-account/download?path=/Developer_Tools/Xcode_14/Xcode_14.zip)

再次运行如下指令
> 若出现错误，请按照错误提示进行执行
```shell
flutter doctor
```

