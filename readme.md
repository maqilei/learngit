## git学习记录

### 基本步骤

1. 首先把当前把当前文件夹交给 git 管理使用`git init`
2. 然后对文件中的内容进行改动，把文件添加到仓库 `git add readme.md`
3. 然后 `git commit -m "新建"`提交到仓库

### 命令记录

1. git status
   可以查看仓库此时的状态，
2. git diff 
   查看改动
3. git log
    查看提交记录
4. git reset --hard HEAD^
   在提交的各个版本之间更换
   回退到上次提交的版本，HEAD代表当前版本，HEAD^代表上一版本
5. git reflog
   查看历史命令
### 删除文件
1. 正常在工作区直接删除后，通过 `git rm 文件名` 然后 `commit`即可
2. 这种情况是在工作区删错了文件，而版本库里还有，需要恢复`git checkout -- test.txt`
### 与远程库关联
1. 把本地库和远程库管理
`git remote add origin git@github.com:maqilei/study.git`
2. 首次提交到远程库`git push -u origin master`, 之后提交 `git push origin master`
### 分支管理
1. 创建分支
使用`git checkout -b dev`表示创建并切换到dev分支，相当于创建分支`git branch dev`和切换分支`git checkout dev` 两个命令的结合
2. 查看分支
`git branch`
3. 合并分支
`git merge dev` 把指定的dev分支合并到当前分支
4. 删除分支
对于已经合并到 master 上的分支，可以直接`git branch -d dev`删除，对于还没有合并到 master 上的分支，需要把参数改为大写的D

### bug 分支（暂存功能)
> 场景：当正在某个分支上开发时，遇到一个bug需要修复，但是开发的工作区的代码还没有完成，需要在一定时间内先解决掉bug，此时就用到了 暂存工作区代码的功能
1. 现在比如master上有个错误需要修复，首先`git stash`暂存当前工作区的代码，此时用`git status`查看当前状态，工作区没有任何变动，
2. 然后假设 bug 发生在 master 分支上，先`git branch master` 切换到master分支，然后创建修改bug分支并切换`git checkout -b bugfix` 
3. 修改完成提交之后，`git merge bugfix`把修改合并到master上，然后可以删除分支
4. 这时候需要回到之前的开发，可以用`git stash apply`恢复，会保存stash记录，还可以用`git stash pop`来恢复stash，类似出栈，stash的内容被删除。可以通过 `git stash list` 查看暂存列表，恢复指定的stash
5. 因为 dev 分支是在修复 bug 之前从 master 分出来的，所以 dev 上的 bug 还在，可以有一个简单的办法，把修改 bug 的提交复制到 dev 分支上，首先切换到 dev 分支，然后执行 `git cherry-pick commit_id`
