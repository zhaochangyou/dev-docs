# dev-docs

###  环境信息

***
- 开发工具　：VS code
- 前端框架：  angular5
- 版本管理：git
- 系统平台：Window7  64Bit
- 后端：.net WCF MVC  WebAPI
***


```
    step 1 : 下载    vscode:https://code.visualstudio.com/insiders/
    根据系统环境选择 x64,x32版本
    安装完成后，启动时会检测 GIT 是否安装
    step 2 : 下载   git :https://git-scm.com/downloads
    默认安装C盘，vs code打开后自动检测 git

    VS Code工具启动完成

    下面就是配置Git帐号和密码，操作脚本可参考 git-bash-demo.md

    新电脑环境快速搭建环境说明：
        下载安装  vscode 和  git 
        终端执行脚本：
        ---全局配置
        git config --global user.name "zhangsan"
        git config --global user.email "test@qq.com"  
        git config --global credential.helper store 
        ---克隆开发库到本地 
        - git clone https://github.com/zhangsan/package.git

```




### 


### vscode  git  clone 过程


- step1:
```
 	login in  https://github.com
 	create  repositories  ->  Create a new repository -> dev-docs  
```
- step2:
```
	创建本地目录文件夹：dev
	使用vs code 工具 打开 dev文件夹

	使用VS CODE自带的初始化存储库
	或
	使用 ctrl+` 组合快捷键进行vs code终端执行以下脚本：
        
        git clone   https://github.com/zhaochangyou/dev-docs.git
    
        终端完成信息如下：

        D:\works\dev>git clone   https://github.com/zhaochangyou/dev-docs.git
        Cloning into 'dev-docs'...
        remote: Counting objects: 3, done.
        remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
        Unpacking objects: 100% (3/3), done.

	检查目录 D:\works\dev\ 克隆库  dev-docs 到本地，同时出现 .git目录 和readme.md 文档

```	
- step3:
```
    编辑 readme.md 文档， ctrl +s  保存，使用组合键  ctrl+shift+G 进入源代码管理窗口  找到readme.md文件 暂存更改  输入commit 备注信息，ctrl + enter  组合键 提交完成  最后打开隐藏菜单，发布分支按钮，操作完成，登录github.com查看更新结果
```	