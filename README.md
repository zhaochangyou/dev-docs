# dev-docs

##### vscode  git  clone 过程


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