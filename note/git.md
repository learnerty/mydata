#### 极简sudo命令
`sudo apt-get install git curl atom`   下载git, curl, atom软件  
`sudo apt-get update`   更新

### 基础git命令
`git clone [url]`   克隆到本地  
`git status`    查看仓库状态  
`git log`     查看仓库日志  
`git pull`   拉取远端更新

##### 本地修改后上传更新  
`git add .`  添加到git 暂时保存下来  
`git commit -m 'message'`  做版本  
`git push`   提交

##### atom 装包命令  
`apm install [packagename]`
```
装的一些常用包
emmet  
open-in-browser  
autocomplete-paths  
language-babel
```

#### 手动创建.gitignore里面写名称,忽略不想上传的文件或文件夹


#### 本地上传
`ssh-keygen`命令，生成电脑钥匙 `.put`公有,将里面内容复制到github的SSH and GPG keys  
`git init` 初始化成一个仓库  
`git remote add origin` 地址  
`git push -u origin master`  提交的master分支

#### 分支
##### 切换分支之前必须全部做成版本才能切换分支  
`git branch` 查看所有分支和当前分支 `*`代表当前分支  
`git branch gh-pages` 创建新的gh-pages分支  
`git checkout gh-pages`  切换到gh-pages分支  
`git push -u origin gh-pages` 向远端推送gh-pages分支  
`git merge gh-pages` 把gh-pages分支上领先的操作拉取过来  
`git branch -d gh-pages` 删除本地gh-pages分支，不能删master分支，并且不能删当前所在分支  
`git push origin :gh-pages`  删除远端的gh-pages分支

`gh-pages`是一个十分特殊的分支，内容可以被访问，地址为learnerty.github.io/仓库名
