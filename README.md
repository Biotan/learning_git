# 一步步学习git的相关操作及多人协作开发
## 模拟环境如下：
机器三台：程序员A在公司的台式机（计算机名：A-company，系统：ubuntu16.04），程序员A在家里的笔记本（计算机名：A-home，系统：windows 10）。程序员B在公司的台式机（计算机名：B-company，系统：ubuntu16.04）  
账户2个：程序员A（账户名：Biotan，注册邮箱：tjingang@mail.ustc.edu.com）；程序员B（账户名：Biotan-tc，注册邮箱：biotan930912@163.com）
# ================= 单人开发篇 ================= 
### ******************************************* 环境配置及异地办公 *******************************************
#### 程序员A和B各自在各自的电脑上配置好ssh的密钥：
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

#### 程序员A创建项目：
创建一个新的远程仓库。登陆github上自己账户，然后点击右上角找到Your Repositories,然后进去点击new，输入名字（以test_git为例），addME不勾选，设置权限（可以是公开的，别人能看不能改，也可以是私有的，别人看不见，这里以公开为例）。  
A用户可以有三种方式来和远程仓库建立连接远程仓库：
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
“git branch -M main”表示将当前分支名改为main，因为github网站上创建的远程仓库现在默认名字为main，而git init创建的默认本地仓库名为master。“git remote add xxx”表示添加远程仓库连接，意思是origin以后就代表xxx，以后提交的时候就不用每次都输入网址了。“git push -u origin main”表示将本地的main分支提交到origin主机的main分支，同时-u表示指定origin为默认主机，后面就可以不加任何参数使用git push了。不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。
（2）如果当前已经有一个写好的工程，则可以直接在改工程下输入如下命令：
```
git remote add origin git@github.com:Biotan/test_git.git
git branch -M main
git push -u origin main
```
(3) 从远程clone下来：
```
git clone https://github.com/Biotan/test_git
cd test_git
echo "# test_git" >> README.md
git add README.md
git commit -m "first commit"
git push
```
这时候省去了设置远程连接的麻烦，不过过程中需要输入创建这个工程的用户名和密码。且clone下来的默认分支就是“main”分支。

#### 程序员A回家后在家办公：
程序员A在公司创建的项目是一个新项目，在家里的电脑上还没有这个项目，所以先把项目clone下来：（！！！建议这一步先不操作，因为这里改动了后面会涉及到冲突合并的问题，这个后续才会说到）
```
git clone https://github.com/Biotan/test_git
cd test_git
echo "work at home " >> README.md
git add README.md
git commit -m "modify README.md"
git push
```
程序员A在家又修改了README.md文件
### *********************************** git简介及基本操作 ************************************
首先我们要明白git这个软件本身的出现就是为了进行代码版本的管理，使得很方便地备份以前地版本，以防老板提出要求回到以前的版本，若是都手动地备份，到后面自己都记不清版本之间的区别（见下图）。git就是帮助你进行版本管理的神器，就相当于完玩单机游戏，记录下各个进度，下次还能回到以前的进度上重新玩。之前提到的在git初始化之后的工程文件夹下面的.git隐藏文件夹就是管理各个版本的仓库。可以把我们的工程文件夹想象成一个显示面板，这个面板上显示什么文件（或者说什么版本的文件），由后台的git来控制，而我们只需要学会控制git去切换就可以了，各个版本的差异git帮我们记住。所以说.git文件里的东西我们不要随意去手动修改（除非你非常熟悉），不然各个版本就乱掉了。  
<img src=img/fig1.png width="400" /><br/>
<img src=img/fig2.png width="400" /><br/>
<img src=img/fig3.png width="400" /><br/>  
一个git管理的项目，都会有一个默认的分支（分支后面讲解），可以理解为记录员，它负责帮你记录各个版本，并负责帮你切换版本。  
git里面有几个概念，前台，缓存区，本地仓库，远程仓库：  
【前台】：就是我们当前能看到的文件夹内部  
【缓存区】：当我们在前台增删改文件后，我们不能直接将这些改动提交到本地仓库，因为你需要告诉管理员（master或者main）你要提交哪些改动，因为并不是所有改动都需要当前提交，比如程序员A今天完成了算法1的编写(algorithm1.py)，算法2(algorithm2.py)的编写完成到一半，这时候老板说先上传算法1给测试的同时区测试，那A就可以先提交algorithm1.py，然后继续写算法2。  
【本地仓库】:你向远程服务器提交的时候，始终是提交的这个本地仓库的文件（当前分支），master（或者main）版运工的工作就是将缓存区的东西搬运到本地仓库，方便你后面向远程提交。  
【远程仓库】：在远程服务器上的仓库，你要提交代码的地方，给别人看，和别人协作，或者用于上线等等  
查看当前有哪些改动：
```
git status  查看当前有哪些改动
```
<img src=img/fig4.png width="400" /><br/>
可以看出新增了algorithm1.py和algorithm2.py这两个文件  

从前台到缓存区的命令【前台】----> 【缓存区】：
```
git add algorithm1.py  将algorithm1.py写入到缓存区
```
<img src=img/fig7.png width="400" /><br/>
可以看出向缓存区写入了一个文件，其实还可以用命令“git add .”来一次性提交所有改动  

从缓存区到本地仓库的命令【缓存区】----> 【本地仓库】：
```
git commit -m "finished algorithm1.py"  告诉当前搬运工将缓存区的东西提交到本地仓库，并让他记录一下这一次操作做了什么，方便以后查看
```
<img src=img/fig5.png width="400" /><br/>
可以看出在本地仓库新加了一个文件。一般好点的规范是直接运行"git commit",这时候会用vim打开一个可编辑文件，在里面按一定的规则明确写清楚改动了哪里，原因是什么等等。    

从本地仓库到远程仓库的命令【本地仓库】----> 【远程仓库】：
```
git push origin main 将本地的main分支的本地仓库提交到远程仓库
```
<img src=img/fig6.png width="400" /><br/>
可以看出成功将本地仓库main提交到了远程仓库main
### ******************************************* 代码版本管理 *******************************************
加入现在A对algorithm1.py进行了修改并重新提交了，然后这时候突然觉得还是修改前的更符合算法逻辑，想回到之前的版本，这时候就体现出git版本管理的强大了
```
git log  查看之前所有提交的版本信息
```
<img src=img/fig8.png width="400" /><br/>
可以看出之前最新的一次操作是对algorithm1.py进行了修改（这就体现除commit的时候注释的重要性了），再往前一次是从远程分支拉取并本地合并（这个暂时先不管，假设这个没有），再往前是创建了algorithm1.py文件，且每个记录都有提交的作者信息，提交版本号，提交时间，提交注释。这时候可以通过以下命令切换到刚创建好algorithm1.py的那次提交的版本，版本号（60af08......）
```
git reset 60af08  版本号不需要输入完全，git会自动搜寻
```
或者也可以用如下命令
```
git reset --hard HEAD^^  HEAD表示最近的一个旧版本，HEAD^^第二旧的版本，一次类推，或者HEAD~5表示第五旧的版本

```
这时候再打开algorithm1.py就会发现是未修改完的初始提交版本了  
这时候再用“git log”查看日志，就会发现如下图所示只能看到比当前切换版本更老的版本了
<img src=img/fig9.png width="400" /><br/>
这时候如果程序员A又反悔了，觉得算法1还是应该改动，又想回到改动后的版本咋办？可以用如下命令查看所有版本：
```
git reflog 站在当前版本时间点，显示过去和将来的所有改动版本

```
<img src=img/fig10.png width="400" /><br/>
可以看出显示出来了所有的版本，然后再用上面的切换命令就可以了


### *************************************** git常用命令 ***************************************
### *************************************** git界面画工具 ***************************************
### ********************************** git分支管理及多人协作 *****************************
git中很重要的一个
### *************************************** git常用配置方法 ***************************************
