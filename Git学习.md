# Git tutorials

This file helps you to use git.

I build a learngit folder in 下载.

[git cheet sheet](https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf)
[廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

## git安装与设置

```
sudo apt-get install git
git --version

# 设置用户名和邮箱
git config --global user.name "happyfan"
git config --global user.email "2290991129@qq.com"
```

## git基本用法

### 初始化git

```
cd /home/happyfan/my——learning/git
git init   # 将文件夹变成git文件夹
```

### Add

```
git add readme.txt   
git add readme.txt readme-copy.txt   # 添加多个文件到暂存区
```

### Commit

```
git commit -m "wrote a readme.txt file"   # 将修改提交到分支
```

### 版本回退

```
# HEAD为当前版本
git reset --hard HEAD^   # 回退到上个版本
git reset --hard HEAD^^   # 回退到上上个版本
git reset --hard 168018   # (或者使用版本号，需要git log或者git reflog得到版本号)
```

### 删除文件

```
rm test.txt   # linux系统删除文件（git中也要删除）

git rm test.txt
git commit -m "remove test.txt"

git checkout -- test.txt   # 误删文件，重新恢复（其实删除也算修改的一种，重新恢复即取消删除这个修改），从未被添加到版本库的文件是无法恢复的
```

### 其他常用命令

```
git status     # 查看git仓库状态
git diff  # 查看文件有哪些更改

git log   # 查看历史提交记录， HEAD表示当前版本
git log --pretty=oneline   # 精简查看提交记录

git reflog  # 查看历史操作和版本号

git checkout -- readme.txt   #撤销文件所有在工作区的修改，但不撤销已提交到暂存区的修改（本质是用版本库的版本替换工作区的版本）
git reset HEAD readme.txt  # 撤销文件所有已提交到暂存区的修改，此时修改存在于工作区（可以使用checkout撤销工作区的修改）
```

### 远程仓库

```
ssh-keygen -t rsa -C "2290991129@qq.com"
github创建仓库learngit
git remote add origin git@github.com:happyprotean/learngit.git
git push -u origin master   # 第一次推送

# 一般先从远程库拉取代码，然后本地解决冲突，最后推送到远程
git pull     # 从远程仓库拉取代码
git push origin master/dev   # 推送代码到远程仓库

git clone git@github.com:happyprotean/learngit.git    # 从github上新克隆一个仓库在当前文件夹
```

## git高级用法

### 分支管理

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

```
git branch dev   # 新建dev分支，dev分支的内容与当前master一致
git checkout dev   # 切换到dev分支工作，所作修改只体现在dev分支
git checkout -b dev  # 等价于上面两条命令

git branch  # 查看当前所有的分支，*表示所在分支
git add
git commit  # dev分支与master分支此时已不同

git checkout master
git merge dev  # 将dev分支合并到当前分支master，dev分支保持不变

git branch -d dev   # 删除dev分支（dev分支已被合并过），建议保留dev分支
git branch -D dev   # 删除dev分支（dev分支未被合并过）
```

- 合并分支时，git在合适的时候采用fast forward模式，但在这种模式下删除分支会丢失掉分支的信息。使用--no-ff可以禁用fast forward模式，在merge时生成新的commit，这样在分支历史上可以看出分支信息。

  ```
  git checkout -b dev
  vim readme.txt
  git add readme.txt
  git commit -m "add merge"
  
  git checkout master
  git merge --no-ff -m "merge with no-ff" dev
  git log --graph --pretty=oneline --abbrev-commit
  ```

  

  ![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

### 分支冲突

当两个分支同时对同一个文件进行修改时，在合并时会存在分支冲突，此时需要手动将合并失败的文件修改，然后在add-commit，会自动的将两个分支的文件合并。

![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

```
cat readme.txt  # 查看文件

git log --graph --pretty=oneline --abbrev-commit  # 查看分支的合并情况
```

### 标签管理

标签本质上是一个指向某个commit的指针。

```
git tag v1.0   # 对当前所在分支最新的commit打tag

git log --pretty=oneline --abbrev-commit    # 查看所有的commit
git tag v0.9 1d8e4   # 对某一次commit打tag
git tag -a v0.9 -m "version 0.9 released" 1d8e4   # 创建带有说明的标签

git show v0.9  #  tag对应commit的信息

git tag -d v0.9  # 删除标签
```

### 忽略特殊文件

使用 .gitnore文件

### 设置别名

```
git config --global alias.st status   # 使用st代替status，即git st
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch

git config --global alias.unstage 'reset HEAD'
```

