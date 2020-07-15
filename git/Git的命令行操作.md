# Git的命令行操作

## 本地库初始化

* 命令：git init
* 注意：.git目录存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改。

## 设置签名

* 形式
  * 用户名
  * Email
* 作用：区分不同开发人员的身份
* 辨析：这里设置的签名和登录远程库（代码托管中心）的账号、密码没有任何关系。
* 命令：
  * 项目级别/仓库级别：仅在当前本地库范围有效
    * git config user.name wangxiansen15
    * git config user.email 1501150926@qq.com
  * 系统用户级别：登录当前操作系统的用户范围
    * git config --global user.name wangxiansen15
    * git config --global user.email 1501150926@qq.com
  * 级别优先级
    * 就近原则：如果二者都有，项目级别优于系统级别
    * 如果都没有，不允许，会报错

## 添加提交以及查看状态操作

### 命令

* git status：查看工作区、暂存区的状态
* git add [file name]：将工作区的“新建/修改”添加到暂存区
* git rm -cached [file]：从暂存区移除该文件
* git commit -m "first change good.txt" [file name]：从暂存区提交到本地库
* git remote -v：查看远程库地址别名
* git remote add origin http://github.com/：将网址添加一个origin的别名
* git push origin master：推送到远程库，git push 仓库地址别名 分支
* git pull origin master：抓取操作