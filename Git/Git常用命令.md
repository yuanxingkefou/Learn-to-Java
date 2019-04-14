#Git常用命令

**检查个人设置**

`git config --list  / git config [key]`

**获取帮助**

```
git help <verb>
git <verb> --help
man git <verb>
```

**在现有目录中初始化仓库**

`git init`

**克隆现有仓库**

`git clone [url]`

**查看当前文件状态**

`git status`

**跟踪新文件，暂存已修改的文件**

`git add [文件名]`
 
**查看已暂存的变更内容**

`git diff --cached`

**提交变更**

`git commit`

**无需暂存，直接提交**

`git commit -a`

**移除文件**

`git rm`

**强制移除已暂存的文件**

`git rm -f`

**查看提交历史**

`git log`

**撤消提交**

`git commit --amend`

**撤消已暂存的文件**

`git reset HEAD [文件名]`

**撤消已修改的文件**

`git checkout -- [文件名]`

**显示远程仓库**

`git remote -v`

**添加远程仓库**

`git remote add [自定义仓库名] [url]`

**从远程仓库中获取数据**

`git fetch [仓库名]`

**将数据推送到远程仓库**

`git push [remoet-name][branch-name]`

**检查远程仓库**

`git remote show [remote-name]`
