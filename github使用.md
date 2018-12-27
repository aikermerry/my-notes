# github 详细使用教程

## 安装客户端

1. 工具 win10，git安装包

2. 百度 github 下载，找到软件下载最新的即可

3. 点击下载的exe文件安装，一直点击next

4. 在Windows Explorer integration选项中将“Git Bash here”和“Git GUI here”打对勾。 

5. 一直到安装成功

[git sign up](https://github.com/aikermerry/my-notes/blob/master/pictures/git%20sign%20up.png)

## 注册github

登录github网站：https://github.com

![git sign up](https://github.com/aikermerry/my-notes/tree/master/pictures\git sign up.png)

输入自己的用户名（自己喜欢的名字），自己的密码（最好是常用的）点击注册就OK啦！

<!--下面的就是重点了-->

## 建立本地仓库

1. 找一个你需要的地方建一个文件夹（我找的是：G:\md）

2. 在文件管理器的框内输入`cmd` 命令

   ![cmd](.\pictures\cmd.png)

2. 配置git，首先在本地创建`ssh key；`

    在命令框内输入`ssh-keygen -t rsa -C "your_email@youremail.com"`

   ```
   G:\md>ssh-keygen -t rsa -C "1139513***@qq.com"
   Generating public/private rsa key pair.
   Enter file in which to save the key (C:\Users\11395/.ssh/id_rsa):
   Created directory 'C:\Users\11395/.ssh'.
   Enter passphrase (empty for no passphrase):
   Enter same passphrase again:
   Your identification has been saved in C:\Users\11395/.ssh/id_rsa.
   Your public key has been saved in C:\Users\11395/.ssh/id_rsa.pub.
   ```

   后面的`your_email@youremail.com`改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在`C:\Users\11395/.ssh/id_rsa`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。 在上面可以看到这条语句

   `Enter file in which to save the key (C:\Users\11395/.ssh/id_rsa)`这里就有你的位置

3. 回到刚才注册的github并登陆自己的帐号，点击右上方的头像

   ![seting](.\pictures\seting.png)

   点击setting后找到左边选择SSH Keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key 

   ![seting](.\pictures\ssh.png)

   找到你的密码文件id_rsa.pub，右键选择打开方式，选择记事本

   复制过去,点击添加就OK了

4. 返回到命令框输入：

   ```
   $ git config --global user.name "your name"
   $ git config --global user.email "your_email@youremail.com"
   ```

5. 在github上创建一个仓库

   ![creat repositories](.\pictures\creat repositories.png)

   ![create2](.\pictures\create2.png)

6. 建立远程连接

   然后复制这个地址用来建立远程连接

   ![remoteUrl](.\pictures\remoteUrl.png)

   ```
   git remote add origin git@github.com:yourName/yourRepo.git
   ```

   好了准备都做好了

7. 上传文件

   在命令窗口输入 `git init`

   ```
   G:\md>git init
   Initialized empty Git repository in G:/md/.git/
   ```

   然后键入`git add <filename> `

   在刚才的文件夹下面创建一个README.md文件然后

   ```
   G:\md>git add README.md
   ```

   使用如下命令以表示提交改动： 

   ```
   git commit -m "代码提交信息" 
   ```

   *引号内顺便填

   推送提交到远程仓库：

   ```
   G:\md>git push -u origin master
   ```

   第一次都要这样，以后就只需要 `git push`就能推上去

   

------

基本教程就到这里吧，其他问题多百度，然后推荐到廖学峰的网站去

网站地址：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000



## 时间12/20

如果在上传工程中遇到问题比如：

```
G:\mymd>git push -u  origin master
To github.com:aikermerry/my-notes.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:aikermerry/my-notes.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

这可能是上传的远程库与本地库版本不一样

解决办法为：

```
方法一：
git pull origin master #将远程库拉倒本地同步，如果不能解决
方法二：
git pull origin master --allow-unrelated-histories #这个是解决可能是版本不同的问题
```

<!--在我们小组的仓库中提交流程大致一样。有什么问题大家要积极搜索网络与想我提问-->

参考链接：

1.[版本问题](https://blog.csdn.net/lindexi_gd/article/details/52554159)

2.[git-分支的新建与合并](https://blog.csdn.net/makenothing/article/details/53014308)



​			
