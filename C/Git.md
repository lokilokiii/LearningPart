- [我的小小经验](#我的小小经验)
  - [大问题](#大问题)
  - [引申：如何迁移master分支到main分支中](#引申如何迁移master分支到main分支中)
  - [引申：在本地切换分支](#引申在本地切换分支)
- [在GitHub中删除已有文件](#在github中删除已有文件)
  - [1. 本地仓库的文件和远程仓库的文件同时删除](#1-本地仓库的文件和远程仓库的文件同时删除)
  - [2. 只删除远程仓库，不删除本地仓库](#2-只删除远程仓库不删除本地仓库)

### **GIT通用操作**

这里的内容非本人整理，仅作笔记学习用途

#### 初始条件

> 1.官网下载git（一路回车）
>
> 2.注册一个Github或者Gitee的账号

#### 添加密钥

> 打开git bash

```bash
cd ~/.ssh/
git config --global user.name 
git config --global user.email 
ssh-keygen -t rsa -C 
```

> 之后一路回车，按照提示的路径找到public key文件，复制内容到GitHub或者Gitee的SSH密钥里

####  初始化仓库

```bash
git init
```

#### Git初始配置

```bash
git config --global user.name 
git config --global user.email 
```

#### 关联远程仓库

```bash
git remote add origin <项目地址>
```

> 这里的项目地址就是你的Github或者Gitee项目主页的SSH或者HTTPS地

#### 更新远程仓库到本地

```bash
git pull origin master(以及任何你想要更新的branches)
```

> 如果新建的远程仓库没有用README.md初始化可以不需要，本质其实就是如果需要推送更改到远程仓库，需要先将本地仓库与远程仓库更新合并。

#### 提交项目

> 保存到缓存区

``` bash
git add . #如果只需要提交某一部分，可以git add 被拖入的项目名
```

> 描述本次提交的内容

```bash
git commit -m "要描述的内容"
```

> 推送到远端仓库上

```bash
git push origin master(以及任何你想要推送的branches)
```

#### 克隆

```bash
git clone <项目地址>
```

> 这个主要是用来克隆整个代码仓库到本地，在本地进行开发

#### 仓库后期推送的注意事项

> 1. 一定要有描述内容
>
> 2. 非必须不要强制提交
>
> 3. 更新仓库前确定本地修改的已经放入缓存区，防止被覆盖
>
> 4. 如果对Github git clone太慢，需要手动开启代理
>
>    ```bash
>    git config --global http.https://github.com.proxy socks5://127.0.0.1:1080 #端口号因人而异
>    git config --global https.https://github.com.proxy socks5://127.0.0.1:1080 
>    ```

#### 多人协作

>  settings -> manage access 选择成员成为collaborators
>
> 然后在接下来的协作中很重要的一点，就是先更新本地仓库，再push，不然是推不上去的。更新的时候，还是那句话，先放入缓存区，再commit，之后再pull，不然自己本地所有的修改都将被覆盖。



## 我的小小经验

这里是我！

### 大问题

这段时间，我想把我自己写的一些学习报告之类的上传到github上保存，然后，我就重新拾起了git操作，弄了一早上，看看我遇到了什么问题然后发现我自己的愚蠢的，我无语住了！！！！！

我之前使用过git，所以我的github账号是和我的电脑已经是连接好了的（公钥已经成功设置了的！！），然后我以为一切都十分简单水到渠成

我创建了我的新一个仓库LearningPart

1. 进入我的本地新目录，打开git bash，初始化git仓库`git init`，初始化设置

   ```shell
   $ git init
   $ git config --global user.name ""
   $ git config --global user.email ""
   $ git remote add origin <仓库ssh>
   ```

2. 初始化完成之后，我就打算add一个文件commit，然后push试试看

   add完之后，commit过程中遇到了<font color="red">**第一个问题**</font>：

   ```shell
   $ git commit 2048.md
   hint: Waiting for your editor to close the file...
   ```

   接着它弹出了一个文件，弹出的内容几乎就与输入命令`git status`一摸一样，就是前面带上**#**

   我关闭了这个文件，接着出现

   ```shell
   Aborting commit due to empty commit message.
   ```

   我尚未发现自己犯了什么愚蠢的错误

   好，到这里，我以为我成功commit了

3. 于是（理所应当的），我继续输入我的指令

   ```shell
   $ git push 2048.md
   ```

   出现了我的<font color="red">**第二个错误**</font>

   ```shell
   fatal: '2048.md' does not appear to be a git repository
   fatal: Could not read from remote repository.
   
   Please make sure you have the correct access rights
   and the repository exists.
   ```

   我只关注到了最后两句话，我以为，我还没连接好:cry:,然后我开始漫漫检查之路

   ```shell
   $ git remote -v
   origin  git@github.com:lokilokiii/LearningPart.git (fetch)
   origin  git@github.com:lokilokiii/LearningPart.git (push)
   #说明我已经成功连接了远程仓库
   
   $ ssh -T git@github.com
   Hi lokilokiii! You've successfully authenticated, but GitHub does not provide shell access.
   #在这里很奇怪，没有提供shell访问？？我以为这里出了问题，找到github官方文件，说应该不是什么问题，出现这句话，说明你的连接已经完成了
   ```

   后来，这里我发现了（假装发现），我应该在前面加上点什么，因为我需要**指定仓库**，而不是胡乱提交，于是我写

   ```shell
   $ git push origin 2048.md
   #origin就是我仓库的ssh，可以根据 $ git remote -v (同时也可以用于检查是否成功连接)查看
   error: src refspec 2048.md does not match any
   error: failed to push some refs to 'github.com:lokilokiii/LearningPart.gi
   ```

   我困惑了???为啥没有，根据提示好像是说他并没有找到这个文件

   好嘛，我现在再看看

   ```shell
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   2048.md
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           "Arduino\345\237\272\346\234\254\347\274\226\347\250\213\350\257\255\346
                                                                                  6\263\225.md"
           Git.md
           String/
           code/
           "\344\275\215\350\277\220\347\256\227.md"
           "\345\206\205\345\255\230\347\256\241\347\220\206/"
           "\345\244\232\346\226\207\344\273\266\347\274\226\347\250\213.md"
           "\346\226\207\344\273\266/"
           "\346\240\274\345\274\217\345\214\226\350\276\223\345\205\245\350\276\22
                                                                                  23\345\207\272\346\213\223\345\261\225.md"
           "\351\242\204\345\256\232\344\271\211\345\256\217.png"
   
   ```

   本来没有看出任何问题，仔细看了之后才发现，**Changes to be committed**？？

   原来，我还没有commit上去，重新回到上一步

4. 重新查看报错信息，这回看懂了

   ```shell
   Aborting commit due to empty commit message.
   ```

   退出了commit因为没有描述内容！！！

   **<font color="blue">解决方法（问题1）</font>**：

   ```shell
   $ git commit -m "hello,i will commit"
   [master e917e94] hello,i will commit
    1 files changed, 256 insertions(+)
    create mode 100644 2048.md
   ```

   * 补充：此处的**`-m`**，指用于提交暂存区的文件，**`git commit -am`**，用于提交跟踪过的文件，就是说，在一个新文件提交的时候，两者都可以已使用，但是如果你修改了一个文件，两者的用法就会有所不同
     1. **`-m`**：对新的文件进行add后再次commit然后push
     2. **`-am`**：直接进行commit然后push就可以了

   **终于成功提交了！！！**

5. 到了push阶段

   ```shell
   $ git push origin 2048.md
   error: src refspec 2048.md does not match any
   error: failed to push some refs to 'github.com:lokilokiii/LearningPart.git'
   ```

   ??怎么还是一摸一样的报错信息，我确定这次已经成功commit上去了

   歪打正着，接下来的**<font color="blue">解决方法（问题2）</font>**：

   ```shell
   $ git push
   fatal: The current branch master has no upstream branch.
   To push the current branch and set the remote as upstream, use
   
       git push --set-upstream origin master
   #系统提醒我输入该指令，没明白，先输入再说   
   $ git push --set-upstream origin master
   Enumerating objects: 15, done.
   Counting objects: 100% (15/15), done.
   Delta compression using up to 16 threads
   Compressing objects: 100% (14/14), done.
   Writing objects: 100% (15/15), 89.14 KiB | 339.00 KiB/s, done.
   Total 15 (delta 0), reused 0 (delta 0), pack-reused 0
   remote:
   remote: Create a pull request for 'master' on GitHub by visiting:
   remote:      https://github.com/lokilokiii/LearningPart/pull/new/master
   remote:
   To github.com:lokilokiii/LearningPart.git
    * [new branch]      master -> master
   Branch 'master' set up to track remote branch 'master' from 'origin'.
   ```

   啊好像成功了一些什么，我就打开我的仓库，我发现文件整整齐齐出现在上面！！

   唯一的不同是，出现了一个新的branch，**master**

   **原来，是我文件中的git采用的是master分支，而github的仓库中设置的是main分支，两者无法同步**，因此应该就本分支创建一个新的分支，成功提交！

6. **<font color="green">就此成功解决问题</font>**

   在已经创建分支后的情况下，我的日常只是需要

   ```shell
   $ git add <file>
   $ git commit -m "messages"
   $ git push
   ```



### 引申：如何迁移master分支到main分支中

> 2020年10月1日后，`Github`会将所有新建的仓库的默认分支从`master`修改为`main`，这就导致了一些旧仓库主分支是`master`，新仓库主分支是`main`的问题，这在有时候会带来一些麻烦，因此这里提供一种方案将旧仓库的`master`分支迁移到`main`分支。

1. **克隆原仓库作为备份**

   ```shell
   git clone XXXX.git
   ```

2. **创建并推送main**

   创建并切换到`main`：

   ```shell
   git checkout -b main
   ```

   推送main：

   ```shell
   git push origin main
   ```

3. **github中设置修改默认主分支**

4. **删除master**

   删除本地master：

   ```shell
   git branch -d master
   ```

   删除远程master：

   ```shell
   git push origin :master
   ```



### 引申：在本地切换分支

```shell
#查看本地分支
$ git branch
#切换分支
$ git checkout -b main
Switched to a new branch 'main'
```



## 在GitHub中删除已有文件

### 1. 本地仓库的文件和远程仓库的文件同时删除

先打开本地仓库的文件夹，选择要删除的文件或者文件夹，点击删除

然后执行

```shell
$ git add *
$ git commit -m "del"
$ git push origin master
```

### 2. 只删除远程仓库，不删除本地仓库

进入到git bash窗口，执行以下命令：

```shell
$ git pull origin master  # 将远程仓库里面的项目拉下来

$ dir # 查看有哪些文件夹

$ git rm -r --cached target  # 删除你要删除的文件名称，这里是删除target文件夹（cached不会把本地的flashview文件夹删除）

$ git commit -m '删除了target' # 提交,添加操作说明

$ git push -u origin master #重新提交（若需要对其他分支进行操作，则把master换为对应分支，如:git push -u origin dev）
```

