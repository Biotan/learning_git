# 一步步学习git的相关操作及多人协作开发
## 模拟环境如下：
机器三台：程序员A在公司的台式机（计算机名：A-company，系统：ubuntu16.04），程序员A在家里的笔记本（计算机名：A-home，系统：windows 10）。程序员B在公司的台式机（计算机名：B-company，系统：ubuntu16.04）  
账户2个：程序员A（账户名：Biotan，注册邮箱：tjingang@mail.ustc.edu.com）；程序员B（账户名：Biotan-tc，注册邮箱：biotan930912@163.com）

## 程序员A和B各自在各自的电脑上配置好ssh的密钥：
配置密钥是为了能本地和github服务器（自己账户）通信
对程序员A而言（A-home电脑）：  
(1)安装git，打开Git Bash  
(2)键入命令：ssh-keygen -t rsa -C "tjingang@mail.ustc.edu.com"，其中邮箱是A自己的github账号邮箱，提醒你输入key的名称，输入如id_rsa，也可以不写，一路回车。  
(3)在A的账户下路径：C:\Users\Tjing\.ssh里面有2个文件：id_rsa和id_rsa.pub，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。  
(4)登陆GitHub网站，右上角个人信息点开，打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title（建议用电脑名称A-home，方便以后账户下很多key的时候知道哪个key对应哪台机器），在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”，你就应该看到已经添加的Key。为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。  
对程序员A而言（A-company电脑）和用户B（B-company电脑）：  和上面一样，只是因为系统变了，文件目录变成了用户主目录：~/.ssh/  
一个github账户可以添加多个key，比如A用户的账户，分别把A-company和A-home的key都添加进去了，以后就可以在家或者在公司提交代码了。
(5) 在计算机上git界面上输入以下命令，表示

## 程序员A创建项目：
（1）第一种方式：创建一个新的项目。登陆github上自己账户，然后点击右上角找到Your Repositories,然后进去点击new，输入名字，设置权限（可以是公开的，别人能看不能改，也可以是私有的，别人看不见，这里以公开为例）。  
（2）第二章中方式：已经有一个代码工程，这时候在工程下面
