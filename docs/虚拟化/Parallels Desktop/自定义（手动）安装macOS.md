### 需求
> `Parallels Desktop` **自动创建**的 `macOS` 虚拟机默认设置的**空间大小（64GB）**不足以支持使用需求，且不支持后续更改，故只能选择**自定义（手动）安装**

### 下载
> 在终端（`terminal`）输入以下信息，并将输出的内容（`URL`）复制到浏览器打开，会自动下载一个 `xx.ipsw` 格式的文件，亦或者通过[官网下载](https://developer.apple.com/download/)（**需要使用Apple ID登录**）
```sh
➜  ~ /Applications/Parallels\ Desktop.app/Contents/MacOS/prl_macvm_create --getipswurl
➜  ~
```

### 安装
> 完成下载后，回到终端（`terminal`）使用以下命令
```sh
/Applications/Parallels\ Desktop.app/Contents/MacOS/prl_macvm_create <path_to_ipsw> <path_to_macVM> --disksize <bytes>
```
- 根据参数位置分别填入
- `<path_to_ipsw>`: 表示下载的 **`xx.ipsw`文件路径**
  > 如：`~/Downloads/UniversalMac_13.5.2_22G91_Restore.ipsw`
- `<path_to_macVM>`: 表示**安装路径**，即创建虚拟机的位置
  > 如：`~/Parallels/macOS.macvm`
- `<bytes>`: 表示设置的**磁盘空间大小**，单位字节（`byte`）
  > 如：需要设置`100GB`，根据换算可得 `100GB = 100 * 1024 * 1024 * 1024 = 107374182400`

  > **需要注意的是**：PD安装的macOS虚拟机**只允许首次设置空间大小**，并**不支持后续更改**，设置需**慎重**

### 进度监测
> 将上述命令正确填入终端（`terminal`）后，敲击回车即进入**安装流程**，此时可以在终端（`terminal`）中看到**安装进度**，当出现`Installation succeeded.`输出时即**安装完成**，如：
```sh
➜  ~ /Applications/Parallels\ Desktop.app/Contents/MacOS/prl_macvm_create /Downloads/UniversalMac_13.5.2_22G91_Restore.ipsw ~/Parallels/macOS.macvm --disksize 107374182400
Starting installation.
Installation progress: 6.00
Installation progress: 8.00
Installation progress: 9.00
Installation progress: 10.00
Installation progress: 11.00
...
Installation progress: 87.00
Installation progress: 90.00
Installation progress: 100.00
Installation succeeded.
➜  ~
```

### 使用（检测）
- 启动 `Parallels Desktop`
- 正常情况下，`PD` 创建的虚拟机都会在 `~/Parallels/` 下，会自动引入
- 如没有自动引入，也可手动将创建的虚拟机拖拽到 `PD` 中，或是通过 `+ -> 打开 -> xx.macvm` 的方式添加
- 启动，并完成初始化设置


### 参考
[Install macOS virtual machine on a Mac with Apple silicon](https://kb.parallels.com/125561)