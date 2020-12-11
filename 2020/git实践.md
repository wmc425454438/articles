# Git实践

## 起因

之前一直用的svn来进行版本管理，所以`git`一直都是自己一个人在练习中使用。

因为自己的项目只有自己在用，`git`的提交流程以及方式都是直接在`master`上进行提交。

现在到了新的公司，都是用`GitLab`来进行提交代码以及版本管理。

需要`git flow`相关的知识以及运用，在网上找了大部分的资料，总结一下具体的分支创建方式以及常用的规范。

## 分支

主要分为五种分支：

- `master` 主分支，只用作发布，不在该分支上直接修改代码。
- `develop` 开发分支，和`master`代码相同，用于合并开发代码和测试人员测试。
- `feature` 特性分支，用于开发人员创建自己的功能分支。从`develop`分支创建，开发完成自测之后可以提交到`develop`分支，删除该分支。
- `bugfifx` bug修复分支，用于修复不紧急的bug。从`develop`分支创建，修复完成后合并到`develop`分支，删除该分支。
- `release` 从develop分支创建，用于测试人员进行测试。如果发现bug立即在`release`分支修复，修复完成后，合并到`develop`分支。
- `hotfix` 用于修复线上紧急bug，从`master`分支创建。修复完成后合并到`master`和`develop`分支。

## 命令（参考）

创建分支
``` bash
git branch develop
```

切换分支

``` bash
git switch develop
// 或者
git checkout develop
```

***切换分之后，再执行分支创建，则是基于该分支创建分支***

创建并切换分支

上面的也可以一句话完成

``` bash
git switch -c develop
// 或者
git checkout -b develop
```

master合并feature上的新特新，删除特性分支
```
git switch master
git merge feature
git branch -d feature
```

一般的merge在删除分支后会丢掉分支信息，这就是所说的merge的fast forward模式。

如果想留住合并时的信息，就不能用此模式，我们可以用--no-ff，这样会在merge时生成一个新的commit。
```
git merge --no-ff -m "merge info" develop
```

合并分支图查看
```
git log --graph
```

如果在开发过程中需要修复一个紧急bug，但是现在又不能直接提交代码。

我们可以用stash方式来暂存当前的代码，等待修复bug后再用stash pop的方式，恢复当前开发现场。

```
git stash
git switch -b bug_fix001
git add readme.txt
git commit -m "bugfix001"
// 生成该次commit的代号 
// [bug_fix001 4c805e2] bugfix001
git switch master
git merge --no-ff -m "bugfix001" bug_fix001
git brach -d bug_fix001
git swtich develop
git stash list
git stash pop
```

可以发现我们在master修复该bug后，develop也有相同的bug。

此时我们只需要merge这个刚修复的commit就可以了，因为develop有可能有新的提交。

这里用到了cherry-pick

```
git swtich develop
git cherry-pick 4c805e2
```

这样就可以把修复的bug提交到develop上了

普通的pull如果有文件更改，会多一次commit的记录。
rebase可以让commit的记录清楚。
```
git pull --rebase
```
