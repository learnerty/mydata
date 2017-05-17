### git
sudo apt-get install git curl atom   下载git, curl, atom软件
sudo apt-get update   更新

git clone [url]   克隆到本地

git status    查看仓库状态

git log     查看仓库日志

修改后上传更新

git add .  添加到git 暂时保存下来

git commit -m 'message'  做版本

git push   提交

atom 装包命令

apm install [packagename]

emmet

open-in-browser
autocomplete-paths
language-babel

手动创建.gitignore里面写名称,忽略不想上传的文件或文件夹


本地上传

ssh-keygen 生成电脑钥匙 .put公有,将里面内容复制到github的SSH and GPG keys

git init 初始化成一个仓库

git remote add origin 地址

git push -u origin master


安装node

先在github上安装nvm  

nvm ls-remote  从远端查看nodejs的版本

nvm install 版本号  下载安装最新的nodejs版本

自动会把npm装上，npm是最大的nodejs的包管理工具

nvm use 版本 切换node的版本

nvm alias default 版本  设置默认的版本

nvm uninstall 版本  卸载nodejs
