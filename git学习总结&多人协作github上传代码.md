# git学习总结&多人协作github上传代码

### 一、安装Git

1. git下载网址[Git - 下载软件包 --- Git - Downloading Package (git-scm.com)](https://git-scm.com/download/win)

2. 安装git，一直下一步就可以，安装成功后在桌面右键鼠标，显示更多选项，选择Git Bash here<img src="C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240910233545251.png" alt="image-20240910233545251" style="zoom: 80%;" />

3. 使用 git -v 可检测是否安装成功。![image-20240910233841544](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240910233841544.png)

4. 注册邮箱

   配置用户名：git config --global user.name "user.name"——用户名可随意输入

   配置邮箱：git config --global user.email "你的邮箱"——邮箱要使用github注册的邮箱

   ![image-20240910234855296](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240910234855296.png)

   ### 二、配置密钥

   1.  首先查看密钥:在git bash输入 cd ~/.ssh
   2. 若出现 "No such file or directory"说明ssh keys还没被设置过。
   3. 创建新的ssh keys：在git中输入 ssh-keygen -t rsa -C "你的邮箱"
   4. 点三次回车，看到如图类似就可以了<img src="C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240910235806835.png" alt="image-20240910235806835" style="zoom: 80%;" />
   5. 完成后通过命令  cat ~/.ssh/id_rsa.pub 得到如图：![image-20240911000152104](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911000152104.png)
   6. 复制 "ssh-rsa AAAA........"，登录GitHub，点击自己右上角头像，选择 Settings - SSH and GPG keys - New SSH key，Title的内容随便，将复制的内容粘贴到Key框内，点击‘ADD SSH key’添加即可。
   7. 检验是否添加成功：在git Bash 中输入：ssh -T git@github.com ——此处就是git@github.com，不是你的邮箱，回车，输入yes,回车，若出现**Hi “用户名”! You’ve successfully authenticated, but GitHub does not provide shell access.**则表示成功辣！

   ### 三、用git将创建本地仓库并上传到远程仓库

   1. 建立一个Git 仓库并对其初始化：可以在某磁盘中建一个 myproject 文件夹，并在其中启动git Bash,

      ![image-20240911003140159](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911003140159.png)

      **git init** ：声明此文件夹为本地仓库。

      **touch README**：创建一个README的文本文件。此时打开myproject文件，发现其中有README文件，对其进行简单编辑，如添加 HelloWorld 字样并保存。

      ![image-20240911003551223](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911003551223.png)

      **git status** ：显示文件状态，标红就表示还没被跟踪。

      **git add .** ：用来跟踪所有文件，也可用 **git add 文件名** 用来追踪具体单个文件，但不推荐使用

      **文件的状态有四种，未修改，已修改，已暂存，未跟踪，总的来说，我们首先要保证文件被跟踪，然后在暂存，每次修改都要再追踪一次。**如我们对README进行修改，这时候查看状态又变红了，所以要再次 add 。

      ![image-20240911004534955](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911004534955.png)

      ![image-20240911010228796](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911010228796.png)

      **git commit -m "文本"**：将文件提交到本地仓库，文本内容可以当作一种日志。

      **git remote add origin https://github.com/WangJiTui/Software-Engineering-Project.git**：建立远程仓库和本地仓库的链接，https后面跟的是远程仓库的地址。

      ![image-20240911010625972](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911010625972.png)

      **git pull --rebase origin master** ：拉取远程仓库master分支的内容。如果远程仓库中有README.md文件，那这步很重要。图中出现了很多 "**fatal:unable ...**"的错误，这个主要是因为网络问题，可用个好点的clash。

      ![image-20240911011058580](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911011058580.png)

      git pull --rebase origin master 成功如图。

      **git push -u origin master** ：将本地仓库内容上传到远程仓库，若远程仓库是空的，则加上-u，下次可直接用 **git push origin master**

      ![image-20240911011413767](C:\Users\97814\AppData\Roaming\Typora\typora-user-images\image-20240911011413767.png)

   
此时仓库中就有我们在本地创建的README文件了，提交记录为“first”，Win!!

### 四、分支管理 -最重要的功能

1. 什么是分支管理：Git 分支管理是 Git 强大功能之一，能够让多个开发人员并行工作，开发新功能、修复 bug 或进行实验，而不会影响主代码库。几乎每一种版本控制系统都以某种形式支持分支，一个分支代表一条独立的开发线。使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。

2. 主分支：在初始化本地 Git 仓库的时候，Git 默认已经帮我们创建了一个名字叫做 master （也有叫main） 的分支。通常我们把这个master 分支叫做主分支。在实际工作中，master （也有叫main）主分支的作用是：用来保存和记录整个项目已完成的功能代码。因此，不允许程序员直接在 master 分支上修改代码，因为这样做的风险太高，容易导致整个项目崩溃。

   #### 由于目前时间有限分支管理内容可以参考

   #### [Git 分支管理 | 菜鸟教程 (runoob.com)](https://www.runoob.com/git/git-branch.html)

   或者

   [(四) github分支的知识_github关于分支-CSDN博客](https://blog.csdn.net/weixin_44098452/article/details/115317258)

   

   

   



​	