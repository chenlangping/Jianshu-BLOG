`git init` 用来初始化当前所在的文件为Git仓库
`git add 你的文件` 把指定的文件添加进仓库
`git commit -m "注释"` commit操作
`git status` 用来显示当前仓库的状态
`git diff 你的文件` 用来显示文件的修改
`git log` 显示从最近到最远的提交日志
`git log --pretty=oneline` 简化输出的日志信息
`git reset --hard HEAD^` 回退到上一个版本
`HEAD`是当前的版本，`HEAD^`是之前的一个版本，`HEAD^^`是之前的两个版本……`HEAD~100`是之前的一百个版本
`git reset --hard commitID` 回退到commitID的那个版本
`git reflog`查看历史版本变化记录
`git checkout -- 你的文件` 丢弃你的文件在**工作区**的修改，即还没有git add之前，其实这个的意思是用版本库中的文件代替你的工作区的文件。
`git reset HEAD 你的文件` 把你在暂存区进行的修改丢弃，然后再用上一条的方法把工作区的修改丢弃
`git rm 你的文件` 从版本库中删除你的文件，记得要commit一下
`git remote add origin git@github.com:用户名/仓库名.git` 把本地内容关联到远程仓库
`git push -u origin master` 第一次推送master分支。以后可以不用加`-u`参数
`git clone github的地址` 利用HTTPS克隆远程仓库，但是慢。
`git clone git@github.com:用户名/仓库名.git` 速度快
`git branch 分支名字` 新建分支
`git checkout 分支名字` 切换到该分支
`git branch` 查看当前分支
`git merge 分支名字` 把当前的分支和目标分支进行合并
`git branch -d 分支名字` 删除分支
`git log --graph` 利用图来显示分支情况

