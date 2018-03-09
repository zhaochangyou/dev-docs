### 帐号设置
```
    git config --global user.name "zhangsan"
    git config --global user.email "test@qq.com"
```
---

### create git 仓库

```
    进入git bash终端
    
    - new git package
    mkdir demo
    cd demo
    git init
    touch readme.md
    git add readme.md
    git commit -m "first commit"
    git remote add origin https://github.com/zhangsan/package.git
    git push -u origin master

    - exist git package

    cd exist_git_repo
    git remote add origin https://github.com/zhangsan/package.git
    git push -u origin master

    - vs code 记录GIT账号和密码命令

    git config --global credential.helper store  //在Git Bash输入这个命令就可以了
   
```