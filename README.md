# git_test

### 配置用户名和email
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
### 创建版本库
```
git init
```

### 添加文件到暂存区
```
git add .
```
### 提交文件到版本库
```
git commit -m""
```
### 查看状态
```
git status
```
### 比较文件
```
git diff <filename>             工作区VS暂存区
git diff head <filename>        工作区VS版本库
git diff --cached <filename>    暂存区VS版本库
```
### 历史
```
git log       提交历史
git reflog    命令历史
```
### 版本回退
```
git reset --hard HEAD^(或版本号)
( HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写成HEAD~100 )

git reset --soft	暂存区->工作区
git reset --mixed	版本库->暂存区
git reset --hard	版本库->暂存区->工作区
```

### 丢弃工作区的修改
```
git checkout -- <filename>
让这个文件回到最近一次git commit或git add时的状态
（一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
）
```

### 删除文件
```
在工作区删除文件        rm file
从版本库删除该文件，命令git rm file,然后git commit
```
### 远程
#### 创建SSH Key
```
ssh-keygen -t rsa -C "youremail@example.com"
```
#### 本地库的所有内容推送到远程库上
```
git push -u origin master
(远程库的名字就是origin
第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令)
```

#### 从远程仓库克隆
```
git clone 地址
```

#### 查看远程库信息
```
git remote -v
```

### 分支
#### 创建分支
```
git checkout -b dev 或 git switch -c dev 创建并切换到dev分支
等同于 
git branch dev  创建分支
git checkout dev 或 git switch dev  切换分支
```

#### 查看当前分支
```
git branch
```

#### 合并指定分支到当前分支
```
git merge <branchname>
```

#### 删除分支
```
git branch -d dev  
git branch -D <name>强行删除没有被合并过的分支
```

#### 查看合并情况
```
git log --graph --pretty=oneline --abbrev-commit
```

#### 普通模式合并
```
git merge --no-ff -m "merge with no-ff" dev
合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
```

### stash
#### 存储工作现场到stash
```
git stash
```

#### 显示stash列表
```
git stash list
```

#### 恢复stash
```
git stash pop
等同于
git stash apply  恢复
git stash drop   删除
```

#### 恢复指定stash
```
git stash apply stash@{0}
```

#### 复制一个特定的提交到当前分支
```
git cherry-pick 4c805e2
```

#### 推送分支
```
git push origin master
```

### 删除远程仓库文件
当我们不错误的把需要ignore的文件上传到了远程仓库时，需要将其删除
```
git rm -r ––cached dirname  //删除远程文件夹，但保留本地文件夹
git commit -m ""           //提交操作，并添加描述
git push origin master     //推送
```

### merge VS rebase
两者都可以合并分支，主要区别是所产生的log tree不一样。
```
原分支情况：
A ---  B ---  C
         -
          --  D  ---  E

git merge后:
A ---  B ---  C  ---------- F
         -              -
          --  D  ---  E

git rebase后:
A ---  B ---  C  --- D' --- E'
```