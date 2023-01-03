### angular 导入json文件

#### 需求

> 使用 `angular` 导入 `json` 文件 

#### 解决方式

> 找到 `tsconfig.json`，并依次找到 `compilerOptions` 配置项，在该配置项下添加以下配置，重启项目即可

```json
"resolveJsonModule":true,      
"esModuleInterop": true,
```
