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

    VS Code 下配置Git帐号和密码：
    操作脚本可参考 git-bash-demo.md
```

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
    编辑 readme.md 文档， ctrl +s  保存，使用组合键  ctrl+shift+G 进入源代码管理窗口  
    找到readme.md文件 暂存更改 
    输入commit 备注信息，
    ctrl + enter  组合键 提交完成  
    最后打开隐藏菜单，发布分支(push)按钮，操作完成，登录github.com查看更新结果
```	
- 新环境快速搭建环境说明：
        ```
            下载安装  vscode 和  git 
        
             终端执行脚本：
        
                git config --global user.name "zhangsan"   ---全局配置
                git config --global user.email "test@qq.com"  ---全局配置
                git config --global credential.helper store ---全局配置
            克隆开发库到本地 
                 git clone https://github.com/zhangsan/package.git
        ```

### vscode angular5 环境设置过程

- step1
```
        安装 node.js  :https://nodejs.org/en/  推荐版本即可
        安装后，系统可以使用 npm 包管理器,angular5 所有使用插件可以手动通过 npm 命令获取
        npm install -g @angular/cli  10分钟左右OK
```        
- step2 
```
        创建angular 应用程序        
        通过终端->进行本地仓库所在目录
        mkdir angular-demo
        cd angular-demo
        ng new app-demo //系统自动创建脚手架demo程序
        通过终端->进行app-demo
        ng serve  //命令 打包成功后 可通过 浏览器查看：http://localhost:4200/
```
### angular5 结构说明

- app-demo
    - src
        - app    //根组件
        - assets //images
        - environments  //环境变更
        - index.html    //serve 访问入口文件
        - main.ts //打包编译入口文件
        - polyfills.ts//加入前缀，保证在特定的浏览器能够正常运行
        - test.ts //单元测试的入口
        - tsconfig.app.json
    - e2e //独立于应用的点对点测试文件夹，包含独立的配置文件
    - package.json //整个应用程序依赖库的配置文件
    - tslint.json //代码检查配置文件，执行ng lint, 团队代码更加规范化
    - tsconfig.json //针对于IDE的type script编译配置文件, IDE会根据这个配置文件提供帮助
    - editorconfig // 编辑工具的基础配置文件，保证每个人在自己的编辑工具中能够拥有相同的基础配置，比如字符，缩进等等
    - angular-cli.json //Angular CLI命令配置文件
    - karma.conf.js //karma //单元测试配置文件，应用于ng test命令
    - protractor.conf.js //针对于protractor点对点测试的配置文件， 执行命令 ng e2e


### 发布常用命令

- webpack打包  npm run build   文件目录： dist 
- 服务启动  npm start   http://localhost:4200
- 网站访问路径   http://localhost:4200/index.html

