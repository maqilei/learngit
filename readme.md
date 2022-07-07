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

### 推送分支
```
git push origin master     // 推送到主分支
```
```
git push origin dev        // 推送到dev分支
```

### 抓取分支
1. 当从远程库上clone时，只能看到master分支需要创建远程的origin到本地
```
git checkout -b dev origin/dev
```
2. 如果项目的一个成员想origin/dev分支推送了他的提交，然后自己也做了修改并做了推送,会推送失败,原因时没有指定本地dev分支与远程`origin/dev`分支的链接
```
git branch --set-upstream-to=origin/dev dev  //设置dev和origin/dev的链接
```
3. 再次 git pull 可以成功，但是有冲突，先修复冲突，然后git pull,然后可能跟当前合并有有冲突，动手解决后，再commit 再 push 

### 多人协作
1. 首先 直接通过 `git push origin <branch_name>`推送自己的修改
2. 如果推送失败，说明有其他成员向远程分支提交了修改，本地落后远程分支，需要先把更新的内容拉取下来，用`git pull`试图合并
3. 如果合并有冲突，则解决冲突，再commit
4. 没有冲突或者解决冲突后，再通过`git push origin <branch_name>`推送，即可成功
5. 另，如果git pull时提示`no tracking information`，则说明本地分支和远程分支没有建立链接关系，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`建立联系。


### 版本
> 给项目打上版本，其和commit绑定再一次，大的版本时某次commit提交后的结果
1. 创建标签
```
git tag v1.0
```
2. 给指定commit 打上tag
```
git tag v0.9 <commit_id>
```

### 拉取远程数据
`git fetch`命令从服务器上抓取本地没有的数据时，只会拉去数据不会把数据与本机合并，而 git pull 时再拉取最新的数据之后还要git merge 合并一下，

### rebase
整合不同分支的修改有两种方法：merge 和 rebase，最简单的合并方法时merge，他会把两个分支的最新修改与他们最近的公共祖先进行三方合并合并生成一个新的结果并提交。而变基操作是把其中一个分支的更改，再另一个分支的最新提交基础上应用一次，即把提价到某一分支上的所有修改都移至另一分支上。
见[详细分析](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)