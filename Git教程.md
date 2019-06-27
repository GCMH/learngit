# Git使用笔记

## 安装Git

[Git下载地址](https://git-scm.com/downloads)

下载后解压安装，在安装目录有一个git-bash.exe，点开弹出一个命令行，说明安装成功。

![1561511050654](imgs\1561511050654.png)



安装好了后还需要设置usernaem和email，在打开的git-bash.exe中输入：

~~~ python
$ git config --global user.name "username"
$ git config --global user.email "xxxx@xxx.com"
~~~



## 创建版本库

版本库就是一个文件夹，该文件下的修改会被Git管理，后续可对该文件夹内的文件进行撤销、还原等操作。

~~~ python
$ mkdir learngit
$ cd learngit
$ pwd
~~~

mkdir dirName代表创建一个名为dirName的文件夹.

cd dirName代表进入名为dirName的文件夹。

pwd代表显示当前路径，进入learngit文件夹后代表显示当前文件夹路径。

![1561534293956](imgs\1561534293956.png)

/代表git-bash.exe所在文件夹，/learngit代表该文件夹与git-bash.exe在同一文件夹（D:/Git/）中。



此时我们已经进入learngit文件夹，接下来就是将当前文件夹设置版本库。

~~~ py
git init
~~~

![1561534527769](imgs\1561534527769.png)

这样就将learngit这个文件夹设置为版本库，可以看到提示初始化空的Git参考，后续是版本库具体路径。

创建成功后，learngi/文件夹下会有一个.git的隐藏文件夹

![1561535914338](imgs\1561535914338.png)

.git是用来跟踪管理版本库的，本身为隐藏文件夹，没事不要修改该文件下内容。



当然设置版本库不一定要在D:/Git文件夹下，可以是任意位置。

![1561534832674](imgs\1561534832674.png)

这里是进入D:\JavaEEIDE\eclipse\MyProject，然后将MyProject文件夹设置为版本库。



### 将文件添加版本库

Git管理修改，只能跟踪文本文件的改动，比如.txt、HTML、程序代码等，对着文件文件可以知道每次修改的具体内容。例如第几行添加了一段代码，删除了哪一行的代码。

但对于二进制文件、如图片只能知道对其进行了修改，但无法知道修改了什么。



Wrod文档是二进制格式的，所以测试时我们采用.txt文件。

使用windows自带的记事本创建.txt文件可能会出现问题，这里为了保险起见，最好使用Notepad++创建。

记得将默认编码格式改为UTF-8.

![1561535448271](imgs\1561535448271.png)

在.txt文件中输入111111，并将其保存到创建的版本库中。本例文件完整路径（D:/Git/learngit/readme.txt)

![1561535655023](imgs\1561535655023.png)



我们进入learngit文件夹，使用 `git status`查看下当前版本库状态。

![1561536350051](imgs\1561536350051.png)

可以看到信息中显示 没有跟踪到文件 readme.txt。

此时readme.txt虽然在版本库中，但是它没有添加或注册到版本库中。



想像一下，将版本库看做一个仓库，readme.txt是一个货物。

此时我们将readme.txt这个货物放在仓库中，没有将其登记在案。

如果这个货物被偷了，我们可以发现它被偷了吗？显然是不能发现的，检查货物时肯定是按照货物的登记表进行检查，它不存在登记表对于仓库管理员来说它就是不可见的，它丢失或者被修改都不会记录在案。





同样我们在版本库中创建了readme.txt文件，但没有将其添加到缓冲区中，这时我们先要将其添加到缓冲区中。



添加到缓冲区使用`git add fileName`命令。

![1561537965473](imgs\1561537965473.png)

添加后状态变成了Changes to be committed

此时只是将文件readme.txt添加到了缓冲区，还没有真正提交。

最后使用`git commit -m "msg"`提交

加上-m 可以输入“msg“每次提交时，可以添加一些说明信息，这样便于管理。

![1561540278259](imgs\1561540278259.png)

提交后可以看做是git为其创建了一个版本，后续可以回退到这个版本。

提交后状态为没有需要提交的修改，而且，工作目录是干净（working tree clean）的。



结合上面的add和commit我们来看下Git状态图：

![Git状态图](https://upload-images.jianshu.io/upload_images/5005907-3279c75fc8af15c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/881/format/webp "Git状态图")



首先文件处于untracked状态，add file后文件就进入了缓存区同时状态变为changes to be commited。

commit后文件就进入了tracked，并且保存了一个版本”file content 1“。



接着上面的例子，我们来温习并巩固下`git add fileName`  `git status` `git commit -m "msg"`

***下列操作结合上述图片理解。***

我们打开readme.txt然后再第二行添加”222222“，此时文件经过edit file进入了modified

此时文件状态为`Change not staged for commit`,代表这个修改（2222222）还没有被提交到缓冲区。

然后使用`git add fileName`添加，添加完毕后再次查看状态`git status`会发现这个修改被添加到了缓存区，状态变为`change to be commit`。

![1561541602644](imgs\1561541602644.png)



![1561541640504](imgs\1561541640504.png)

此时修改（222222）被提交到了缓冲区，最后使用`git commit -m "file content12"`就又保存了一个版本。

此时有两个版本`file content1` `fiel content12`

查看所有版本可通过`git reflog`命令查看

![1561542700344](imgs\1561542700344.png)

可以看到有 `file content1` 和 `file content12` 

HEAD代表当前版本，当前版本为`file content 12` 



## 版本回退

上面我们已经创建了两个版本，我们依葫芦画瓢在创建一个版本，这次在文本中添加一行“333333”和一行“444444”。

![1561542409568](imgs\1561542409568.png)

接着使用`git add fileName`然后`git commit -m "msg"`,这时我们有这样一个需要。

在每次进行`git add fileName` 或`git commit -m "msg"`知道修改了那些内容，

先执行`git add fileNmae`然后采用`gie diff fileNme`查看修改的内容。

![1561543275348](imgs\1561543275348.png)



看以看到修改的数据被标识出来了（22222被标记出来是因为22222这一行后面添加了一个回车），但是`git add fileName`之后再使用`git diff fileNmae`显示没有修改的内容。

这里的`git diff fileNmae` 是当前文件与前一次`git add fileName`或 `git commit -m "msg"`

之后的文件内容相比较，只有在`git add fileName`或 `git commit -m "msg"`后对文件进行修改，然后调用`git diff fileName`才会显示变化的内容。

上例中`git add fileNmae`后没有修改文件内容，此时执行`git diff fileNmae`就会显示没有变化（不同）。



也可以换一个角度理解，`git diff fielNmae` 显示的是`Changes not staged for commit`状态的信息，

例如`commit`后进行修改，这个修改处于`Changes not staged for commit`状态会被显示出来，再执行`add`

后进入`Changes to be committed`,所以`diff`不会显示。再次修改，这个修改是`Changes to be committed`状态，所以会被`diff`显示出来。



将这几个修改都提交，后我们查看下有哪些版本。



![1561547692948](imgs\1561547692948.png)

目前有`file content1` `file content12` `file content1234`这三个版本，当前版本为`file content1234`.

~~~ latex
111111
222222
333333
444444
~~~



现在我们想回到`file content1`这个版本，可以采用`git reset -hard HEAD^`

一个`^`代表回退一个版本（还原到fiel contene12）， 两个`'^^'`代表回退两个版本（还原到file content1），

如果有要回退1000个版本怎么办，不可能写1000个`^`吧，这时可以采用`HEAD~X`x代表回退多个版本，为1代表回退一个版本，为2代表会退2个版本。

![1561548161787](imgs\1561548161787.png)

我们使用`git reset --hard HEAD~2`回退两个版本。

![1561548802001](imgs\1561548802001.png)

可以看到，HEAD is now at 92408d file content 1，显示了当前版本内容。

`cat fileNmae`可查看文件内容，`file content1`的内容是“111111”说明回退成功。



我们回退到`file content1`之后我们还想回到`file content1234`应该怎么办呢？

这时可以采用`git reset --hard 提交序号`还原到指定版本。

通过`git reflog` 命令显示的最前面的``92f408d`就是提交序号。

每个人的提交号都不一样，以自己实际的提交序号为准。

~~~ python
92f408d (HEAD -> master) HEAD@{0}: reset: moving to HEAD~2
e07d912 HEAD@{1}: commit: file content1234
bd8ba2d HEAD@{2}: commit: file content 12
92f408d (HEAD -> master) HEAD@{3}: reset: moving to HEAD
92f408d (HEAD -> master) HEAD@{4}: reset: moving to HEAD
92f408d (HEAD -> master) HEAD@{5}: commit (initial): file content 1
~~~



我们需要还原到`file content1234` 使用`git reset --hard 92f408d `即可，版本号可用tab补齐。

![1561550013835](imgs\1561550013835.png)

`cat fileName`参考文件内容为（111111 22222）,代表成功恢复到指定版本。



## 工作区和缓存区

之前在将文件添加到版本库中有所介绍，每一个修改有其状态，没有被add的修改是`Changes not staged for commit`状态，在工作区中。add之后的修改是`Changes to be committed`状态在缓存区。

![Git状态图](https://upload-images.jianshu.io/upload_images/5005907-3279c75fc8af15c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/881/format/webp "Git状态图")

目前文件版本是`file content12`,我们做一个测试步骤如下：

1. 先使用`cat file`Name查看文件内容

2. 使用`vim fileNmae`进入编辑页面，按`i`进入编辑模式在文件中添加333333，

   编辑完毕按`Esc`然后按`:`接着输入`wq`wq代表保存退出。，

3. 使用`cat fileName`查看文件，

4. 然后使用`git add fileName`将这个修改添加到缓存区，

5. 然后再向文件中添加444444，

6. 使用`cat fileName`查看文件，

7. 然后`git commit -m "msg"`提交缓冲区中的修改，

8. 然后使用`git status`查看状态。

![1561551656496](imgs\1561551656496.png)

第一次修改被提交了，但第二次修改还处于`Changes not staged for commit`状态在工作区，此时第二个修改位于图片中的`modified`，第一个修改位于`stage`.而`git commit -m "msg"`只提交`stage`中的修改。

***注意一点Gitg管理的是每一个操作，此处的操作是一个广泛的概念，向文件中添加内容是一个操作，删除文件中内容也是一个操作，删除版本库中的文件也是一个操作，在版本库中做的所有操作都会被Git管理起来***





## 撤销操作

### 撤销修改

撤销修改可以丢弃工作区的修改，命令：`git checkout -- fileName`。

这里撤销有两种情况：

`git commit -m "msg"`后我们修改了文件内容，发现修改错误，这时可以撤销修改，回到commit时的状态

`git add fileName`后修改文件内容，发现修改错误，这时可以撤销修改，回到add时的状态.

为了测试这个两种情况我们先来创建一个``revoke.txt`文件，在其中写入111111。

然后使用`git add fileName`将其添加到版本库。

![1561554221380](imgs\1561554221380.png)

首先我们创建了一个文件，便在其中添加111111，此时文件处于`untracked`状态，我们使用`git add fileNmae`添加文件，使其处于`change to be committed`状态在缓冲区中。



![1561554419807](imgs\1561554419807.png)

然后我们使用`vim`修改文件内容，新加一行222222，然后查看修改状态，有两个修改操作，一个是之前在文件中写入111111，这个修改已经add了，处于缓存区中。而第二个修改在文件中添加222222处于`change not staged for commit`状态，在工作区中。



![1561555357451](imgs\1561555357451.png)

接下来使用`git checkout -- fileName` 撤销修改，然后查看状态，只保留了第一次提交的111111，第二次提交的22222被撤销了，文件回到了上一次add时的状态。

回顾下这一节的第一句话**撤销修改可以丢弃工作区的修改**，第二个修改位于工作区，所有它被撤销了。



接着上述操作，来看另外一种情况：commit后，修改文件，撤销修改。

![1561555607556](imgs\1561555607556.png)

首先commit，然后修改文件内容，再文件中添加一行22222.查看文件状态，添加222222这个修改处于`change not staged for commit`状态，在工作区中。



接下来我们使用`git checkout -- revoke.txt`，撤销工作区内容。显然修改会被撤销，文件内容会变为111111，文件回到了上一次commit时的状态。

![1561555854582](imgs\1561555854582.png)

`git checkout -- fileName`可以撤销工作区中的修改。

由上述分析出的结论：

执行`git checkout -- fileName`后，文件会回到最近一次`add`或`commit`时的状态。



### 撤销删除

不需要的文件可以通过`rm fileNmae`进行删除。

前面说到了，Git可以管理各种操作，删除也是一个操作，而操作是可以撤销的。

![1561558988903](imgs\1561558988903.png)

1. 首先我们创建一个`test.txt`

2. 将文件添加到版本库

3. 删除文件

4. 查看状态

   查看状态我们会发现，将文件到版本库这个操作由于执行了`add`所以在缓存区，虽然文件缺失被删除了，但删除操作`rm fileNmae`依然在工作区还没有提交到缓存区。

5. 显示当前文件夹文件列表

   可以发现`test.txt`确实是被删除了。



结合前面学的`git checkout -- fileName`可以撤销工作区的操作，而`rm`也是一个操作，而且该操作在工作区，所以可以撤销。

注：这里文件以及被删除了，所以`tab`不会自动补齐，需要手动输入文件名。

![1561559463661](imgs\1561559463661.png)

我们撤销了删除操作，然后查看文件夹下文件，发现有`text.txt`文件。



到这里可能会有疑惑，使用`rm fileName`删除文件后，当前操作被保留到工作区，那如何将删除操作提交呢？

先使用`rm fileNmae`删除文件后，在使用`git rm fileName`

![1561559854264](imgs\1561559854264.png)

还是先删除文件，删除操作还没有被提交到缓冲区，执行`git rm fileName`，再次查看状态会发现 删除操作被提交了。后续继续`commit`就可以保存当前版本了。

除了使用`git rm` 还可以使用`add -u`提交所有修改和被删除操作，`add`命令还可以跟随一些参数，具体参数及含义可在网上查阅。

![1561560498614](imgs\1561560498614.png)

## 远程仓库