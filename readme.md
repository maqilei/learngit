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