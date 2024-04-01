### 通过DNS配置指定仓库并解析自定义域名

### 需求

> 通过DNS解析将自己的域名与`Github Page`自定义域名进行绑定

- 在任何页面的右上角，单击个人资料照片，然后单击“设置”。

- 在边栏的“代码、规划和自动化”部分中，单击“ Pages”。

- 在右侧，单击“添加域”。

- 在“要添加什么域？”下，输入要验证的域，然后选择“添加域”。
  > 如：这里要添加`example.com`

- 按照“添加 DNS TXT 记录”下的说明，使用域托管服务创建 TXT 记录。
  > 去到`域名代理商`解析控制后台，添加记录集，并将`1`的内容作为`主机记录`，`2`的内容作为`值`

- 等待您的 DNS 配置更改，这可能是立即更改或最多需要 24 小时。 可以通过在命令行上运行 dig 命令来确认对 DNS 配置的更改。 在以下命令中，将 USERNAME 替换为你的用户名，将 example.com 替换为要验证的域。 如果您的 DNS 配置已更新，您应该会在输出中看到新的 TXT 记录。
  ```sh
  dig _github-pages-challenge-USERNAME.example.com +nostats +nocomments +nocmd TXT
  ```

### 重点🏁

> 若要同时配置多个自定义域名（一个域名指向一个仓库-repository）
- 只需要在`域名代理商`控制台添加`CNAME`记录集合`blog.exmpale.com`，同时指向`你的GitHub名称.github.io`，如：`xx.github.io`
- 在所指向仓库中创建一个`CNAME`文件（不需要任何后缀）
- 将自定义域名添加到该文件中，如：`blog.example.com`
- 在仓库的`设置 setting` -> `Pages` -> `Custom Domain`下填写`blog.example.com`并点击保存，等待一会儿(此时可以在仓库查看`Actions`，部署完成)就可以访问了
- 此处建议同时将`Enforce HTTPS`选项选上，站点将默认使用`https`访问，提高安全性


### 参考

[验证 GitHub Pages 的自定义域](https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)

[GitHub Pages 自定义域名实践整理](https://blog.csdn.net/weixin_34195546/article/details/88016594?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-88016594-blog-131685791.235%5Ev43%5Epc_blog_bottom_relevance_base5&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-88016594-blog-131685791.235%5Ev43%5Epc_blog_bottom_relevance_base5&utm_relevant_index=2)
