### 第一次使用命令行的git工具
生成秘钥对略过。
添加公钥到Github也很简单略过。
配完之后用`ssh -T git@github.com`这条命令看看是不是能成功连接上，它说`hi......`就说明OK了。
然后是创建仓库`git init`
随便创建点测试数据，基本操作。接着去GitHub上创建仓库。
最后关联两者
`git remote add origin https://github.com/yourname/test.git`
`git push -u origin master`
就可以收工了，那个`-u`是为了绑定。
之后就可以简单的使用`git push`进行操作了
如果你有客户端那是最轻松快捷的。

### 最常见的操作
我在我的Git的文件内新建了一个Markdown的文件，然后写好内容。之后开始`git add 我的文件.md` 将它加入暂存区。`git commit -m "第一篇Markdown文件"` 提交到仓库。最后一步`git push`推送到远程仓库就收工了。就是我一次很简单的git操作。

### 推送大文件
某天下午我想把老师的课件也上传到GitHub，方便后人。但是GitHub是不推荐你把50M以上的文件push上去的，而且禁止100M以上的文件上传的。而我这边有一份文章就是220M的，就去百度了一下，还好GitHub提供了大文件的服务。
![github LFS服务](http://upload-images.jianshu.io/upload_images/13050335-f76ee16ec9eb5384.gif?imageMogr2/auto-orient/strip)
1.安装 
我是mac系统，直接执行如下命令安装:
> brew install git-lfs

2.进入本地仓库目录初始化LFS
> git lfs install

3.用git lfs管理大文件 
用git lfs track命令跟踪特定后缀的大文件，或者也可以直接编辑.gitattributes，类似于.gitignore文件的编写，在此我只处理pdf文件：
> git lfs track "*.pdf"

然后将.gitattributes文件添加进git仓库：
>git add .gitattributes

4.接下来就可以像平时使用git那样正常使用了，可以将大文件提交到GitHub了

git add 1.pdf 
git commit -m "commit"
git push 

### 删除几次commit
我发现了我有一次commit不小心把一些不该上传的文件上传了。第一想法是立马在本地进行删除，然后再来一次Git的提交操作。然而，GitHub上面的记录并没有被删掉，也就是说还是可以看到这些不该上传的文件。没办法，只能上网搜了一下。
记录一下，首先是查看日志`git log --pretty=oneline` 找到我错误操作的那几步。然后开始回退`git reset --hard HEAD^^`  <--我这里是回退了2个版本到正确的部分，然后是直接`git push --force`即可。这时候到github上一看记录就没了。注意这时候本地还是可以还原的，如果你确定没有问题了，而又不想记录那些多余的数据，你可以直接在本地删除这个仓库然后在GitHub上pull 一份。如果发现自己回退了又后悔了，可以用`git reflog`查看每次的commit id，然后用`git reset --hard commitid`来返回。注意这里你返回了还需要`git push --force`来让GitHub也返回之前的状态，这里注意你的commit 时间就是你之前做的时间。

### 删除文件操作
这个很重要。
我直接用`rm -rf file`删除了一个文件，然后Git会检测到然后告诉我。如果我真的要删掉，那就用`git rm file`来从版本库中删除；如果是误删，就用`git checkout -- file`来恢复。就算你真的把它删了，你还是可以通过版本回退来得到它。。。所以你其实是删不掉的，要完全删除一个文件，你还需要把它的记录也一并删除，这个就不涉及了。

### 分支工作并且合并分支
主要是需要在分支上进行一些修改的工作。首先创建分支`git branch dev`，然后切换到该分支上面`git checkout dev`，进行自己的工作并且进行commit。然后回到主分支上进行工作，`git checkout master`，之后就是把两者合并--`git merge dev`。如果顺利的话git会处理好一切，如果它解决不掉，你可以通过`git status`查看那些它解决不掉的文件，手动编辑，然后再`add`和`commit`即可

### 推送分支
今天向GitHub推送了自己的分支，用的指令是`git push --set-upstream origin dev`。这个时候Github上面就会多一个branch出来，然后告诉你可以生成pull request，这个时候点击，你就可以生成了，然后按照它的提示，要么网页版改正，要么就是用命令行解决。都有相应的提示，merge完成之后就会有相应的提示了。
