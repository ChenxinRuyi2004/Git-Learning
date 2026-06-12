# Git / GitHub / Gitee 从 0 到上手

这份教程的目标不是让你背命令，而是让你真正理解项目管理的日常流程：

```text
我改了文件 -> Git 记录这次修改 -> 上传到 GitHub 或 Gitee -> 别人或另一台电脑可以拉取更新
```

## 1. 你先理解三个东西

### 1.1 Git 是什么

Git 是一个版本管理工具。它可以记录一个文件夹里每一次重要修改。

你可以把它理解成游戏存档：

```text
存档 1：项目刚开始
存档 2：完成登录页面
存档 3：修复登录 bug
存档 4：增加用户设置
```

以后如果写坏了，就能回到之前的存档。

### 1.2 GitHub / Gitee 是什么

Git 是本地工具，GitHub 和 Gitee 是远程代码托管平台。

区别可以这样理解：

```text
Git：你电脑里的版本管理工具
GitHub：国外常用代码托管平台
Gitee：国内常用代码托管平台，访问速度通常更友好
```

你可以把同一个项目同时上传到 GitHub 和 Gitee。

### 1.3 一个 Git 项目的四个区域

```text
工作区       你正在修改的文件
暂存区       git add 后，准备提交的内容
本地仓库     git commit 后，本机保存的版本历史
远程仓库     git push 后，GitHub / Gitee 上的版本
```

日常流程：

```bash
git status
git add .
git commit -m "说明这次做了什么"
git push
```

## 2. 第一次使用 Git

### 2.1 查看 Git 是否安装

```bash
git --version
```

如果能看到版本号，说明已经安装。

### 2.2 设置你的名字和邮箱

Git 每次提交都需要知道是谁提交的。

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

查看配置：

```bash
git config --global user.name
git config --global user.email
```

当前这台电脑已经配置过：

```text
user.name = ChenxinRuyi2004
user.email = S.S.W2004@outlook.com
```

## 3. 把普通文件夹变成 Git 项目

进入项目文件夹：

```bash
cd "D:\Codex\Git Learning"
```

初始化 Git：

```bash
git init
```

初始化后，文件夹里会出现一个隐藏的 `.git` 文件夹。它就是 Git 保存版本历史的地方。

查看状态：

```bash
git status
```

常见输出含义：

```text
Untracked files    新文件，Git 还没有开始管理
Changes not staged 修改了文件，但还没有 add
Changes to be committed 已经 add，准备 commit
working tree clean 没有未提交的修改
```

## 4. 第一次提交

把所有文件加入暂存区：

```bash
git add .
```

提交：

```bash
git commit -m "初始化 Git 学习仓库"
```

查看提交历史：

```bash
git log --oneline
```

你会看到类似：

```text
a1b2c3d 初始化 Git 学习仓库
```

这里的 `a1b2c3d` 是提交 ID，后面回退版本会用到。

## 5. 日常修改文件的完整流程

假设你修改了 `demo/hello.txt`。

第一步，看状态：

```bash
git status
```

第二步，看具体改了什么：

```bash
git diff
```

第三步，加入暂存区：

```bash
git add demo/hello.txt
```

也可以一次加入所有修改：

```bash
git add .
```

第四步，提交：

```bash
git commit -m "练习：修改 hello 文件"
```

第五步，看历史：

```bash
git log --oneline --graph
```

## 6. `.gitignore`：哪些文件不要交给 Git

有些文件不应该提交，例如：

```text
临时文件
编译产物
日志
系统缓存
密码配置文件
node_modules
```

这些可以写进 `.gitignore`。

示例：

```gitignore
node_modules/
dist/
.env
*.log
```

注意：`.gitignore` 只对还没有被 Git 跟踪的文件生效。如果一个文件已经提交过，再写进 `.gitignore` 不会自动取消跟踪。

取消跟踪但保留本地文件：

```bash
git rm --cached 文件名
```

## 7. 回退、撤销、救命命令

这是新手最容易害怕的部分。先记住一句话：

```text
没 push 的历史，可以比较自由地 reset。
已经 push 给别人看的历史，优先用 revert。
```

### 7.1 文件改坏了，还没有 add

查看：

```bash
git status
git diff
```

丢弃某个文件的修改：

```bash
git restore 文件名
```

例子：

```bash
git restore demo/hello.txt
```

注意：这个命令会丢掉你还没提交的修改。

### 7.2 已经 add 了，但不想提交

取消暂存：

```bash
git restore --staged 文件名
```

然后如果连修改也不要：

```bash
git restore 文件名
```

### 7.3 commit 写错了，想改提交说明

如果只是最后一次 commit 的说明写错：

```bash
git commit --amend -m "新的提交说明"
```

如果这个 commit 已经 push 到远程，并且别人可能已经拉取，改历史前要谨慎。

### 7.4 想撤销最后一次 commit，但保留文件修改

```bash
git reset --soft HEAD~1
```

效果：

```text
commit 没了
修改还在
修改仍处于已暂存状态
```

### 7.5 想撤销最后一次 commit，并取消 add，但保留文件修改

```bash
git reset --mixed HEAD~1
```

效果：

```text
commit 没了
修改还在
需要重新 git add
```

### 7.6 想彻底回到上一个 commit

```bash
git reset --hard HEAD~1
```

效果：

```text
commit 没了
文件修改也没了
```

危险：这个命令会丢数据。敲之前先运行：

```bash
git status
git log --oneline
```

### 7.7 已经 push 了，想撤销某次提交

推荐：

```bash
git revert 提交ID
```

它不会删除历史，而是新增一个“反向提交”。团队协作中更安全。

## 8. 分支：为什么要用 branch

分支适合用来做新功能、修 bug、试验想法。

查看分支：

```bash
git branch
```

新建并切换分支：

```bash
git switch -c feature/practice
```

提交一些修改后，切回主分支：

```bash
git switch main
```

合并分支：

```bash
git merge feature/practice
```

删除已经合并的分支：

```bash
git branch -d feature/practice
```

常见分支命名：

```text
main                 主分支
dev                  开发分支
feature/login        登录功能
fix/login-error      修复登录错误
docs/git-guide       文档修改
```

## 9. 冲突：多人改了同一处怎么办

冲突通常发生在：

```text
你改了某一行
别人也改了同一行
你 pull 或 merge 时 Git 不知道该保留谁的
```

冲突文件里会出现：

```text
<<<<<<< HEAD
你的版本
=======
别人的版本
>>>>>>> 分支名或提交ID
```

解决方法：

1. 打开文件
2. 手动改成你想要的最终内容
3. 删除 `<<<<<<<`、`=======`、`>>>>>>>`
4. 保存
5. 执行：

```bash
git add 冲突文件
git commit
```

如果冲突来自 `git pull`，解决后也通常是 `git add` 再 `git commit`。

## 10. 连接 GitHub

### 10.1 在 GitHub 创建仓库

1. 打开 GitHub
2. 点击 New repository
3. Repository name 填项目名，例如 `git-learning`
4. 如果本地已经有 README，远程就不要勾选初始化 README
5. 创建仓库

### 10.2 本地连接 GitHub 仓库

GitHub 会给你一个地址，形如：

HTTPS：

```text
https://github.com/你的用户名/git-learning.git
```

SSH：

```text
git@github.com:你的用户名/git-learning.git
```

添加远程地址：

```bash
git remote add origin https://github.com/你的用户名/git-learning.git
```

确认：

```bash
git remote -v
```

第一次上传：

```bash
git push -u origin main
```

以后上传：

```bash
git push
```

### 10.3 GitHub 登录方式

现在 GitHub HTTPS 推送通常需要 Personal Access Token，不是账号密码。

更推荐新手使用 SSH，配置一次后比较省心。

生成 SSH key：

```bash
ssh-keygen -t ed25519 -C "你的邮箱"
```

一路回车即可。

查看公钥：

```bash
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

复制输出内容，到 GitHub：

```text
Settings -> SSH and GPG keys -> New SSH key
```

测试：

```bash
ssh -T git@github.com
```

看到欢迎信息就成功。

## 11. 连接 Gitee

### 11.1 在 Gitee 创建仓库

1. 打开 Gitee
2. 点击新建仓库
3. 仓库名称填 `git-learning`
4. 如果本地已经有 README，远程不要初始化 README
5. 创建仓库

### 11.2 本地连接 Gitee 仓库

HTTPS 示例：

```text
https://gitee.com/你的用户名/git-learning.git
```

SSH 示例：

```text
git@gitee.com:你的用户名/git-learning.git
```

如果这个项目只上传 Gitee：

```bash
git remote add origin https://gitee.com/你的用户名/git-learning.git
git push -u origin main
```

如果已经有 GitHub 的 `origin`，可以给 Gitee 单独起名：

```bash
git remote add gitee https://gitee.com/你的用户名/git-learning.git
git push -u gitee main
```

### 11.3 Gitee SSH 配置

如果已经生成过 SSH key，可以直接复用公钥。

查看公钥：

```bash
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

复制到 Gitee：

```text
头像 -> 设置 -> SSH 公钥
```

测试：

```bash
ssh -T git@gitee.com
```

## 12. 同时推送到 GitHub 和 Gitee

推荐做法：给两个远程仓库不同名字。

```bash
git remote add github git@github.com:你的用户名/git-learning.git
git remote add gitee git@gitee.com:你的用户名/git-learning.git
```

第一次推送：

```bash
git push -u github main
git push -u gitee main
```

以后推送：

```bash
git push github main
git push gitee main
```

查看远程：

```bash
git remote -v
```

如果地址写错了：

```bash
git remote set-url github 新地址
git remote set-url gitee 新地址
```

删除远程：

```bash
git remote remove github
```

## 13. 从远程更新本地

最常用：

```bash
git pull
```

它相当于：

```bash
git fetch
git merge
```

更稳一点的习惯：

```bash
git fetch
git status
git log --oneline --graph --decorate --all
git pull
```

如果你本地有未提交修改，先提交或暂存：

```bash
git add .
git commit -m "保存本地修改"
git pull
```

临时不想提交，可以 stash：

```bash
git stash
git pull
git stash pop
```

## 14. 克隆别人的项目

从 GitHub 或 Gitee 复制仓库地址，然后：

```bash
git clone 仓库地址
```

示例：

```bash
git clone https://github.com/用户名/项目名.git
```

进入项目：

```bash
cd 项目名
```

查看远程：

```bash
git remote -v
```

## 15. Pull Request / Merge Request 是什么

GitHub 通常叫 Pull Request，简称 PR。

Gitee 通常叫 Pull Request，也有人叫合并请求。

它的意思是：

```text
我在一个分支里做了修改，请求把我的修改合并到主分支。
```

常见团队流程：

```text
main 主分支保持稳定
从 main 新建 feature 分支
在 feature 分支开发
push 到远程
网页上创建 PR
别人 review
通过后合并进 main
```

命令示例：

```bash
git switch main
git pull
git switch -c feature/add-note
```

修改文件后：

```bash
git add .
git commit -m "添加学习笔记"
git push -u origin feature/add-note
```

然后去 GitHub / Gitee 网页上创建 PR。

## 16. 一个完整演示案例

下面这套练习建议你真的敲一遍。

### 案例 A：提交一个学习笔记

新建文件：

```bash
ni exercises/day1.txt
```

写入内容：

```text
今天学会了：
1. git status
2. git add
3. git commit
```

查看状态：

```bash
git status
```

提交：

```bash
git add exercises/day1.txt
git commit -m "添加 day1 学习笔记"
```

看历史：

```bash
git log --oneline --graph
```

### 案例 B：修改文件并查看差异

修改 `exercises/day1.txt`，多加一行：

```text
4. git log
```

查看差异：

```bash
git diff
```

提交：

```bash
git add exercises/day1.txt
git commit -m "补充 git log 笔记"
```

### 案例 C：撤销未提交修改

随便改 `demo/hello.txt`。

查看：

```bash
git diff
```

如果不想要这次修改：

```bash
git restore demo/hello.txt
```

### 案例 D：建立新分支开发

```bash
git switch -c feature/day2-note
```

新建：

```bash
ni exercises/day2.txt
```

写点内容，然后提交：

```bash
git add exercises/day2.txt
git commit -m "添加 day2 学习笔记"
```

回到主分支并合并：

```bash
git switch main
git merge feature/day2-note
git branch -d feature/day2-note
```

### 案例 E：上传到 GitHub 或 Gitee

先在 GitHub 或 Gitee 网页创建一个空仓库。

如果用 GitHub：

```bash
git remote add github git@github.com:你的用户名/git-learning.git
git push -u github main
```

如果用 Gitee：

```bash
git remote add gitee git@gitee.com:你的用户名/git-learning.git
git push -u gitee main
```

如果两个都用：

```bash
git push -u github main
git push -u gitee main
```

## 17. 新手最常见问题

### 17.1 为什么我 push 失败

常见原因：

```text
远程地址写错
没有登录权限
SSH key 没配置
远程仓库不是空的
本地分支名不是 main
```

排查：

```bash
git remote -v
git branch
git status
```

### 17.2 main 和 master 是什么

它们都是分支名。现在新项目通常用 `main`。

如果你的分支叫 `master`，想改成 `main`：

```bash
git branch -M main
```

### 17.3 每次提交说明怎么写

提交说明要说清楚“做了什么”。

好例子：

```text
添加登录页面
修复用户头像上传失败
补充 Git 回退教程
```

不好的例子：

```text
更新
修改
111
```

### 17.4 什么时候 commit

建议一个小目标一个 commit：

```text
完成一个功能
修复一个 bug
补充一段文档
调整一个配置
```

不要一天只提交一次巨大的 commit。

## 18. 你现在最应该背下来的 12 个命令

```bash
git init
git status
git add .
git commit -m "说明"
git log --oneline --graph
git diff
git restore 文件名
git switch -c 分支名
git switch main
git merge 分支名
git pull
git push
```

## 19. 推荐学习路线

第一天：

```text
git init
git status
git add
git commit
git log
git diff
```

第二天：

```text
git restore
git reset
git revert
```

第三天：

```text
branch
switch
merge
解决冲突
```

第四天：

```text
GitHub 建仓库
Gitee 建仓库
push / pull
SSH key
```

第五天：

```text
PR / MR
团队协作流程
复盘常见错误
```

## 20. 每次操作前的安全习惯

在执行不熟悉的命令前，先敲：

```bash
git status
git log --oneline --graph --decorate --all -n 10
```

如果命令里有下面这些词，要特别谨慎：

```text
--hard
clean
reset
rm
force
```

尤其是：

```bash
git reset --hard
git clean -fd
git push --force
```

它们不是不能用，而是要知道后果再用。

