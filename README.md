# 一步步学习git的相关操作及多人协作开发
## 模拟环境如下：
机器三台：程序员A在公司的台式机（计算机名：A-company，系统：ubuntu16.04），程序员A在家里的笔记本（计算机名：A-home，系统：windows 10）。程序员B在公司的台式机（计算机名：B-company，系统：ubuntu16.04）  
账户2个：程序员A（账户名：Biotan，注册邮箱：tjingang@mail.ustc.edu.com）；程序员B（账户名：Biotan-tc，注册邮箱：biotan930912@163.com）

## 程序员A和B各自在各自的电脑上配置好ssh的密钥：
配置密钥是为了能本地和github服务器（自己账户）通信
对程序员A而言（A-home电脑）：  
(1)安装git，打开Git Bash，执行以下命令进入：
```
cd ~
```
查看是否有.ssh隐藏文件夹，如果没有可以新建一个  
(2) 生成ssh公钥和密钥
```
mkdir .ssh
cd .ssh
ssh-keygen -t rsa -C "tjingang@mail.ustc.edu.cn"
```
请把以上命令的用户名替换成自己的账户名，邮箱是A自己的github账号邮箱，提醒你输入key的名称（随便），也可以不写，一路回车。 密码可以设置也可以不设置，设置的话以后每次在这台电脑上提交代码时都需要输入密码。 这时候在.ssh文件夹里有2个文件：id_rsa和id_rsa.pub，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。    
(3)登陆GitHub网站，右上角个人信息点开，打开“Account settings”，“SSH and GPG Keys”页面，然后，点“Add SSH Key”，填上任意Title（建议用电脑名称A-home，方便以后账户下很多key的时候知道哪个key对应哪台机器），在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”，你就应该看到已经添加的Key。为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。（注意windows系统中你的当前命令行路径在哪儿，生成的密钥文件就在哪儿，因此如果没有提前进入.ssh文件夹，则需要将这2个文件复制到.ssh文件夹下面）
(4) 使用如下命令查看ssh-key是否配置成功
```
ssh -T git@github.com
```

(5) 在计算机上git界面上输入以下命令：
```
git config --global user.name "Biotan"
git config --global user.email "tjingang@mail.ustc.edu.cn"
```
设置git自己的名字和电子邮件。这是因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。注意git config命令的–global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址，这时候不要加global关键字参数。可以使用如下命令查看当前的配置信息：
```
git config -l
```
对程序员A（A-company电脑）和程序员B（B-company电脑）：  
和以上操作一样，一个github账户可以添加多个key，比如A用户的账户，分别把A-company和A-home的key都添加进去了，以后就可以在家或者在公司提交代码了。

## 程序员A创建项目：
创建一个新的远程仓库。登陆github上自己账户，然后点击右上角找到Your Repositories,然后进去点击new，输入名字（以test_git为例），addME不勾选，设置权限（可以是公开的，别人能看不能改，也可以是私有的，别人看不见，这里以公开为例）。  
A用户可以有两种方式来和远程仓库建立连接远程仓库：
(1) 通过clone的方式，输入如下命令：
```
mkdir test_git
cd test_git
echo "# test_git" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:Biotan/test_git.git
git push -u origin main
```
这里先创建了一个同名文件夹，然后新建了一个文件README.md，"git init"表示初始化，用git来跟踪当前工程的版本，这时候当前文件夹会生成一个.git文件夹负责记录各个分支以及各个版本信息，一般不动它。“git add README.md”表示将当前改动提交到缓存区，git commit -m "first commit"表示将缓存区的改动提交到本地仓库（默认是master），并写上一些注释。
“git branch -M main”表示将当前分支名改为main，因为github网站上创建的远程仓库现在默认名字为main，而git init创建的默认本地仓库名为master。“git remote add xxx”表示添加远程仓库连接，意思是origin以后就代表xxx，以后提交的时候就不用每次都输入网址了。“git push -u origin main”表示将本地的main分支提交到origin主机的main分支，同时-u表示指定origin为默认主机，后面就可以不加任何参数使用git push了。
