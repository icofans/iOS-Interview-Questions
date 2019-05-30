# 持续集成
## 1.你在项目中使用过什么持续集成方式？

- Fastlane：一套用Ruby写的自动化工具集，可用于iOS和Android的打包、发布，节省了大量时间。Fastlane配置比较简单，主要编写集成的lane，然后在命令行操作即可

- Jenkins：Jenkins比较受欢迎，插件众多，但对新手来说配置可能稍微麻烦点。

## 2.jenkins怎么备份恢复

- 只需要拷贝主home下面的 .jenkins打个包，下次要恢复就用这个覆盖，所有的东西就都一模一样了。其实就是配置的东西都在这里面，插件的话有个Plugin的文件夹下面就是所有的插件的东西。

## 3.jenkins你都用了哪些插件？

- Keychains and Provisioning Profiles Management：管理本地的keychain和iOS证书的插件

- Xcode integration：用于xcode构建

- GIT plugin/SVN：代码管理插件

