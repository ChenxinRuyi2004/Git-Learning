# Git 学习记录 - 从本地到远程的必会手册

这份笔记整理的是本项目中已经实际练过的 Git 知识点。目标不是背命令，而是让你真正知道：

```text
我现在在哪个区域？
这条命令会改变哪个区域？
运行后应该看到什么结果？
如果出错应该怎么办？
```

## 0. 你已经完成的学习路线

我们前面实际练过这些内容：

```text
1. Git 是什么，GitHub/Gitee 是什么
2. 工作区、暂存区、本地仓库、远程仓库
3. git status 查看状态
4. git diff 查看未暂存修改
5. git add 放入暂存区
6. git diff --staged 查看已暂存修改
7. git commit 保存到本地仓库历史
8. git log 查看提交历史
9. git restore 撤销工作区修改
10. git restore --staged 取消暂存
11. branch/switch/merge 分支和合并
12. conflict 冲突产生和解决
13. .gitignore 忽略文件
14. remote/push/pull 连接和同步 GitHub
15. clone 下载远程仓库
16. fork + pull request 参与别人的项目
17. 常见错误和处理方法
```

## 1. Git 四个区域

Git 最重要的是先分清四个区域。

```text
工作区       你电脑文件夹里真实看到、正在编辑的文件
暂存区       git add 后，准备进入下一次 commit 的内容
本地仓库     git commit 后，本机保存的版本历史
远程仓库     git push 后，GitHub/Gitee 上的版本
```

日常主流程：

```text
工作区修改文件 -> git add -> 暂存区 -> git commit -> 本地仓库 -> git push -> 远程仓库
远程仓库有更新 -> git pull -> 本地同步更新
```

### 1.1 工作区

工作区就是你电脑中的项目目录，比如当前项目：

```text
D:\Codex\Git Learning
```

你用记事本、VS Code 或命令行修改文件，都是在改工作区。

### 1.2 暂存区

暂存区可以理解成“下一次提交的购物车”。

```bash
git add demo/hello.txt
```

意思是：

```text
把 demo/hello.txt 当前这份修改放进暂存区，准备提交。
```

### 1.3 本地仓库

本地仓库就是 `.git` 文件夹中保存的提交历史。

`.git` 不是打开 Git Bash 自动生成的，它来自两种情况：

```text
1. 在普通文件夹里运行 git init
2. 用 git clone 下载远程仓库
```

`.git` 里面保存：

```text
提交历史
分支信息
暂存区信息
远程仓库地址
Git 配置
```

不要手动修改 `.git` 文件夹。

### 1.4 远程仓库

远程仓库就是 GitHub 或 Gitee 上的项目副本。

```text
本地 commit -> git push -> 上传到远程
远程有更新 -> git pull -> 拉回本地
```

## 2. Git 命令的通用结构

Git 命令通常长这样：

```bash
git 动作 参数 文件名
```

例如：

```bash
git commit -m "第二次练习"
```

逐词解释：

```text
git       调用 Git 这个工具
commit    执行“提交”动作
-m        message 的缩写，表示后面直接写提交说明
"第二次练习" 这次提交的说明文字
```

再比如：

```bash
git push -u origin main
```

逐词解释：

```text
git       调用 Git
push      上传本地提交到远程
-u        建立本地分支和远程分支的追踪关系
origin    远程仓库别名
main      要上传的分支名
```

## 3. 基础检查命令

### 3.1 `git --version`

作用：

```text
查看 Git 是否安装，以及安装的版本。
```

命令：

```bash
git --version
```

逐词解释：

```text
git        调用 Git
--version  显示版本信息
```

典型结果：

```text
git version 2.54.0.windows.1
```

说明 Git 已经安装成功。

### 3.2 `git config --global user.name`

作用：

```text
设置或查看 Git 提交时使用的用户名。
```

设置：

```bash
git config --global user.name "ChenxinRuyi2004"
```

查看：

```bash
git config --global user.name
```

逐词解释：

```text
git        调用 Git
config     修改或查看 Git 配置
--global   全局配置，对当前电脑上的所有 Git 仓库生效
user.name  提交者名字
```

### 3.3 `git config --global user.email`

作用：

```text
设置或查看 Git 提交时使用的邮箱。
```

设置：

```bash
git config --global user.email "S.S.W2004@outlook.com"
```

查看：

```bash
git config --global user.email
```

逐词解释：

```text
git         调用 Git
config      修改或查看配置
--global    全局配置
user.email  提交者邮箱
```

### 3.4 `git config --global core.quotepath false`

作用：

```text
让 Git 正常显示中文文件名，而不是显示八进制转义乱码。
```

命令：

```bash
git config --global core.quotepath false
```

逐词解释：

```text
git             调用 Git
config          修改配置
--global        全局生效
core.quotepath  是否把特殊路径转义显示
false           不转义，尽量直接显示中文
```

我们遇到过这种显示：

```text
"\346\210\221\347\232\204\350\207\252\350\277\260"
```

设置后会显示为：

```text
我的自述
```

## 4. 创建 Git 仓库

### 4.1 `git init`

作用：

```text
把一个普通文件夹变成 Git 仓库。
```

命令：

```bash
git init
```

逐词解释：

```text
git   调用 Git
init  initialize 的缩写，初始化仓库
```

运行结果：

```text
当前文件夹中出现隐藏目录 .git
这个文件夹开始被 Git 管理
```

注意：

```text
打开 Git Bash 不会自动生成 .git
cd 进入目录也不会生成 .git
只有 git init 或 git clone 会生成 .git
```

### 4.2 `git clone`

作用：

```text
从 GitHub/Gitee 下载一个已有远程仓库到本地。
```

命令：

```bash
git clone https://github.com/ChenxinRuyi2004/Git-Learning.git
```

逐词解释：

```text
git       调用 Git
clone     克隆，也就是完整下载仓库
https://... 远程仓库地址
```

结果：

```text
创建一个新文件夹
下载项目文件
自动创建 .git
自动配置 origin
自动拉取默认分支
```

记忆：

```text
本地普通文件夹变 Git 仓库：git init
已有远程仓库下载到本地：git clone
```

## 5. 查看当前状态

### 5.1 `git status`

作用：

```text
查看当前仓库状态。
```

命令：

```bash
git status
```

逐词解释：

```text
git     调用 Git
status  状态
```

常见结果 1：

```text
On branch main
nothing to commit, working tree clean
```

含义：

```text
当前在 main 分支
没有需要提交的修改
工作区是干净的
```

常见结果 2：

```text
Changes not staged for commit:
        modified: demo/hello.txt
```

含义：

```text
文件改了
修改还在工作区
还没有 git add
```

常见结果 3：

```text
Changes to be committed:
        modified: demo/hello.txt
```

含义：

```text
修改已经 git add
已经进入暂存区
下一次 commit 会保存它
```

常见结果 4：

```text
Untracked files:
        notes.txt
```

含义：

```text
notes.txt 是新文件
Git 还没有开始追踪它
```

常见结果 5：

```text
deleted: 我的自述
```

含义：

```text
这个文件以前被 Git 追踪过
但现在工作区里没有它了
Git 认为它被删除了
```

## 6. 查看修改差异

### 6.1 `git diff`

作用：

```text
查看工作区和暂存区之间的差异。
```

命令：

```bash
git diff
```

逐词解释：

```text
git   调用 Git
diff  difference 的缩写，查看差异
```

核心含义：

```text
git diff = 看还没 add 的修改
```

典型结果：

```diff
+没错，就是世界上最帅的帅哥。
```

含义：

```text
+ 表示新增内容
```

如果看到：

```diff
-旧内容
+新内容
```

含义：

```text
- 表示旧内容被删除
+ 表示新内容被添加
合起来就是这一行被替换了
```

### 6.2 `git diff --staged`

作用：

```text
查看暂存区和上一次提交 HEAD 之间的差异。
```

命令：

```bash
git diff --staged
```

逐词解释：

```text
git       调用 Git
diff      查看差异
--staged  staged 表示暂存区，查看已经 git add 的内容
```

核心含义：

```text
git diff --staged = 看已经 add、准备 commit 的修改
```

如果运行后没有输出：

```text
暂存区没有变化
没有准备提交的内容
```

### 6.3 `git diff HEAD`

作用：

```text
查看当前工作区相对于上一次提交，总共变了什么。
```

命令：

```bash
git diff HEAD
```

逐词解释：

```text
git   调用 Git
diff  查看差异
HEAD  当前分支的最新提交
```

含义：

```text
不管有没有 git add，都能看从上一次提交到现在的总变化。
```

### 6.4 三个 diff 的区别

```text
git diff
工作区 vs 暂存区
看还没 add 的修改

git diff --staged
暂存区 vs 上一次提交
看已经 add、准备 commit 的修改

git diff HEAD
工作区 vs 上一次提交
看当前总变化
```

## 7. 暂存修改

### 7.1 `git add 文件名`

作用：

```text
把指定文件的修改放入暂存区。
```

命令：

```bash
git add demo/hello.txt
```

逐词解释：

```text
git             调用 Git
add             添加到暂存区
demo/hello.txt  要暂存的文件路径
```

运行后：

```bash
git status
```

可能看到：

```text
Changes to be committed:
        modified: demo/hello.txt
```

含义：

```text
这个文件的修改已经准备提交。
```

### 7.2 `git add .`

作用：

```text
把当前目录下所有变化放入暂存区。
```

命令：

```bash
git add .
```

逐词解释：

```text
git  调用 Git
add  添加到暂存区
.    当前目录，以及当前目录下的文件和子目录
```

注意：

```text
git add . 很方便，但要先 git status 看清楚，避免把不想提交的文件也 add 进去。
```

### 7.3 `git add` 也能暂存删除动作

如果状态是：

```text
deleted: 我的自述
```

运行：

```bash
git add 我的自述
```

含义不是“把文件加回来”，而是：

```text
把“我的自述 被删除了”这个变化放入暂存区。
```

然后：

```bash
git commit -m "删除我的自述"
```

就会把删除动作保存到本地仓库历史。

## 8. 提交修改

### 8.1 `git commit -m "提交说明"`

作用：

```text
把暂存区里的内容保存成本地仓库的一次提交。
```

命令：

```bash
git commit -m "第二次练习"
```

逐词解释：

```text
git        调用 Git
commit     提交，把暂存区内容保存到本地仓库历史
-m         message 的缩写，后面直接写提交说明
"第二次练习" 这次提交的说明
```

典型结果：

```text
[main 4bc7da9] 第二次练习
 1 file changed, 1 insertion(+)
```

逐行解释：

```text
main
提交发生在 main 分支

4bc7da9
这次提交的短 ID

第二次练习
提交说明

1 file changed
有 1 个文件变化

1 insertion(+)
新增了 1 行
```

### 8.2 提交信息怎么写

好的提交信息应该说明“这次做了什么”。

推荐：

```text
练习：修改 hello 文件
练习：提交第二部分
删除我的自述
修复登录页面按钮样式
新增用户配置说明
```

不推荐：

```text
111
改了
asdf
最终版
```

### 8.3 中文引号问题

错误示例：

```bash
git commit -m "修改文件“
```

前面是英文引号：

```text
"
```

后面是中文弯引号：

```text
“
```

结果 Git Bash 会进入续行模式：

```text
>
```

处理：

```text
按 Ctrl + C 取消
重新输入英文半角引号
```

正确：

```bash
git commit -m "修改文件"
```

## 9. 查看提交历史

### 9.1 `git log`

作用：

```text
查看提交历史。
```

命令：

```bash
git log
```

逐词解释：

```text
git  调用 Git
log  日志，查看提交记录
```

可以看到：

```text
提交 ID
作者
时间
提交说明
```

### 9.2 `git log --oneline`

作用：

```text
一行显示一个提交，比较清爽。
```

命令：

```bash
git log --oneline
```

逐词解释：

```text
git        调用 Git
log        查看历史
--oneline  one line，一个提交显示成一行
```

注意：

```text
正确是 --oneline
不是 --online
```

错误示例：

```bash
git log --online --graph
```

结果：

```text
fatal: unrecognized argument: --online
```

### 9.3 `git log --oneline --graph`

作用：

```text
用图形方式显示提交关系。
```

命令：

```bash
git log --oneline --graph
```

逐词解释：

```text
git        调用 Git
log        查看历史
--oneline  一行一个提交
--graph    用图形线条显示分支关系
```

典型结果：

```text
* 61932f5 提交第二部分
* 98497de 只提交一部分
* 4bc7da9 第二次练习
* 3876e0d 修改文件
* 3c00927 初始化 Git 学习仓库
```

含义：

```text
越上面越新
越下面越旧
每一行代表一次 commit
前面的短 ID 后面可以用来查看、恢复或撤销
```

### 9.4 `git log --oneline --graph --decorate --all`

作用：

```text
查看所有分支和远程分支的提交图。
```

命令：

```bash
git log --oneline --graph --decorate --all
```

逐词解释：

```text
git         调用 Git
log         查看历史
--oneline   一行一个提交
--graph     显示图形线条
--decorate  显示分支名、HEAD、远程分支名
--all       显示所有分支，而不是只显示当前分支
```

我们项目中出现过类似：

```text
* 71a8da3 (HEAD -> main, feature/practice-branch) 练习，在分支上修改hello
* 61932f5 提交第二部分
```

含义：

```text
HEAD -> main 表示当前站在 main 分支
feature/practice-branch 表示这个分支也指向同一次提交
```

## 10. 撤销和恢复

### 10.1 `git restore 文件名`

作用：

```text
丢弃工作区中还没暂存的修改。
```

命令：

```bash
git restore demo/hello.txt
```

逐词解释：

```text
git             调用 Git
restore         恢复
demo/hello.txt  要恢复的文件
```

适用场景：

```text
文件改坏了
还没 git add
想恢复到当前最新提交的样子
```

运行结果：

```text
工作区修改被丢弃
git status 变成 working tree clean
git diff 没有输出
```

注意：

```text
git restore 文件名 会删除你未保存进 Git 的工作区修改。
确认不要这次修改时再用。
```

### 10.2 `git restore --staged 文件名`

作用：

```text
取消暂存，但保留工作区文件内容。
```

命令：

```bash
git restore --staged demo/hello.txt
```

逐词解释：

```text
git             调用 Git
restore         恢复
--staged        操作暂存区
demo/hello.txt  要取消暂存的文件
```

运行前：

```text
Changes to be committed:
        modified: demo/hello.txt
```

运行后：

```text
Changes not staged for commit:
        modified: demo/hello.txt
```

含义：

```text
修改还在文件里
但已经不准备提交了
```

### 10.3 两个 restore 的区别

```text
git restore --staged 文件名
只取消 git add
保留文件内容

git restore 文件名
丢弃工作区修改
文件内容恢复到当前提交
```

### 10.4 `git restore --source=提交ID 文件名`

作用：

```text
从某个旧提交中取出某个文件，恢复到工作区。
```

命令：

```bash
git restore --source=4bc7da9 demo/hello.txt
```

逐词解释：

```text
git              调用 Git
restore          恢复
--source=4bc7da9 从这个提交 ID 取文件
demo/hello.txt   要恢复的文件
```

含义：

```text
不是让整个仓库穿越回过去
只是把旧版本的这个文件拿出来覆盖工作区
之后你可以再 git add 和 git commit
```

### 10.5 `git revert 提交ID`

作用：

```text
用一个新的提交，撤销某个旧提交带来的变化。
```

命令：

```bash
git revert 625c804
```

逐词解释：

```text
git      调用 Git
revert   反向撤销
625c804  要撤销的提交 ID
```

适合场景：

```text
某个提交已经 push 到远程
不想改历史
想用一个新的提交把它抵消掉
```

## 11. 分支

### 11.1 分支是什么

分支可以理解成：

```text
从当前版本开一条新路线，在新路线里修改，不影响 main 主线。
```

常见用途：

```text
开发新功能
修复 bug
做实验
参与团队协作
创建 Pull Request
```

### 11.2 `git branch`

作用：

```text
查看本地分支。
```

命令：

```bash
git branch
```

逐词解释：

```text
git     调用 Git
branch  分支
```

结果：

```text
* main
```

含义：

```text
当前只有 main 分支
* 表示当前所在分支
```

如果看到：

```text
  feature/practice-branch
* main
```

含义：

```text
有两个分支
当前在 main
```

### 11.3 `git switch -c 分支名`

作用：

```text
创建新分支，并切换到新分支。
```

命令：

```bash
git switch -c feature/practice-branch
```

逐词解释：

```text
git                     调用 Git
switch                  切换分支
-c                      create 的缩写，创建
feature/practice-branch 新分支名字
```

运行结果：

```text
Switched to a new branch 'feature/practice-branch'
```

含义：

```text
新分支创建成功，并且已经切换过去。
```

### 11.4 `git switch 分支名`

作用：

```text
切换到已有分支。
```

命令：

```bash
git switch main
```

逐词解释：

```text
git     调用 Git
switch  切换
main    目标分支名
```

运行结果：

```text
Switched to branch 'main'
```

注意：

```text
切换分支时，Git 会把工作区文件切换成对应分支的版本。
```

我们验证过：

```text
main 上没有 feature 分支新增的那一行
切回 feature 分支后，那一行又出现
```

### 11.5 `git merge 分支名`

作用：

```text
把指定分支的修改合并到当前分支。
```

命令：

```bash
git merge feature/practice-branch
```

逐词解释：

```text
git                     调用 Git
merge                   合并
feature/practice-branch 要合并进来的分支
```

重点：

```text
先切到“接收成果”的分支
再 merge“提供成果”的分支
```

例如：

```bash
git switch main
git merge feature/practice-branch
```

意思是：

```text
把 feature/practice-branch 合并到 main。
```

### 11.6 Fast-forward

合并时出现：

```text
Fast-forward
```

含义：

```text
main 没有自己的新提交
feature 分支只是在 main 前面多走了几步
所以 Git 直接把 main 指针往前移动
```

合并前：

```text
main                    -> 61932f5
feature/practice-branch -> 71a8da3
```

合并后：

```text
main                    -> 71a8da3
feature/practice-branch -> 71a8da3
```

### 11.7 `git branch -d 分支名`

作用：

```text
删除已经合并完的本地分支。
```

命令：

```bash
git branch -d feature/practice-branch
```

逐词解释：

```text
git                     调用 Git
branch                  分支管理
-d                      delete 的缩写，删除
feature/practice-branch 要删除的分支名
```

运行结果：

```text
Deleted branch feature/practice-branch (was 71a8da3).
```

含义：

```text
删除的是分支名字，不是删除提交，也不是删除文件。
只要 main 已经指向那次提交，提交仍然存在。
```

## 12. 冲突

### 12.1 冲突是什么

冲突就是：

```text
两个分支改了同一个文件的同一处位置
Git 不知道应该保留哪一个
所以让人手动决定
```

### 12.2 触发冲突的典型过程

```bash
git switch -c feature/conflict-demo
# 修改 demo/hello.txt 的某一行
git add demo/hello.txt
git commit -m "练习：conflict 分支修改同一行"

git switch main
# 修改 demo/hello.txt 的同一行，但改成另一种内容
git add demo/hello.txt
git commit -m "练习：main 分支修改同一行"

git merge feature/conflict-demo
```

典型结果：

```text
CONFLICT (content): Merge conflict in demo/hello.txt
Automatic merge failed; fix conflicts and then commit the result.
```

含义：

```text
自动合并失败
需要手动修改冲突文件
```

### 12.3 冲突文件长什么样

文件中会出现：

```text
<<<<<<< HEAD
这是 main 分支上的版本。
=======
这是 conflict-demo 分支上的版本。
>>>>>>> feature/conflict-demo
```

含义：

```text
<<<<<<< HEAD 到 =======
当前分支的内容，也就是你 merge 时站着的分支

======= 到 >>>>>>>
要合并进来的分支内容
```

### 12.4 怎么解决冲突

解决冲突本质就是：

```text
直接打开文件，手动改成最终想要的内容。
```

例如改成：

```text
这是解决冲突后的最终版本：main 和 conflict-demo 的修改都看过了。
```

然后删除冲突标记：

```text
<<<<<<< HEAD
=======
>>>>>>> feature/conflict-demo
```

最后：

```bash
git add demo/hello.txt
git commit -m "解决 hello 文件冲突"
```

这里的 `git add` 含义是：

```text
告诉 Git：这个冲突文件已经解决好了。
```

## 13. .gitignore

### 13.1 `.gitignore` 是什么

`.gitignore` 是一个普通文本文件，用来告诉 Git：

```text
哪些文件不要追踪，不要提交。
```

它在当前项目中已经存在。

### 13.2 查看 `.gitignore`

命令：

```bash
cat .gitignore
```

逐词解释：

```text
cat         输出文件内容
.gitignore 要查看的文件
```

也可以用：

```bash
ls -a
```

逐词解释：

```text
ls  列出文件
-a  all，显示隐藏文件
```

### 13.3 修改 `.gitignore`

用记事本：

```bash
notepad .gitignore
```

用 VS Code：

```bash
code .gitignore
```

一行一条规则。

常见写法：

```gitignore
# 忽略某个具体文件
notes.txt

# 忽略所有 .log 文件
*.log

# 忽略某个文件夹
node_modules/

# 忽略环境变量文件
.env
.env.*

# 不忽略某个文件，感叹号表示例外
!important.log
```

### 13.4 `.gitignore` 的重要规则

```text
.gitignore 只影响还没有被 Git 追踪的文件。
```

如果文件已经被 commit 过，后来再写进 `.gitignore`，Git 仍然会继续追踪它。

如果想让 Git 不再追踪，但本地保留文件：

```bash
git rm --cached 文件名
```

逐词解释：

```text
git       调用 Git
rm        remove，移除
--cached  只从 Git 索引中移除，不删除本地文件
文件名     要停止追踪的文件
```

### 13.5 检查文件为什么被忽略

命令：

```bash
git check-ignore -v test.log
```

逐词解释：

```text
git           调用 Git
check-ignore  检查忽略规则
-v            verbose，显示详细信息
test.log      要检查的文件
```

结果会告诉你：

```text
是哪一个 .gitignore 文件的哪一行忽略了这个文件。
```

## 14. 远程仓库

### 14.1 `git remote -v`

作用：

```text
查看当前仓库连接了哪些远程仓库。
```

命令：

```bash
git remote -v
```

逐词解释：

```text
git     调用 Git
remote  远程仓库
-v      verbose，显示详细地址
```

没有远程时：

```text
没有输出
```

配置远程后：

```text
origin  https://github.com/ChenxinRuyi2004/Git-Learning.git (fetch)
origin  https://github.com/ChenxinRuyi2004/Git-Learning.git (push)
```

含义：

```text
origin 是远程仓库别名
fetch 是拉取用的地址
push 是上传用的地址
```

### 14.2 `git remote add origin 地址`

作用：

```text
给本地仓库添加远程仓库地址。
```

命令：

```bash
git remote add origin https://github.com/ChenxinRuyi2004/Git-Learning.git
```

逐词解释：

```text
git       调用 Git
remote    管理远程仓库
add       添加
origin    远程仓库别名，默认常用名
https://... 远程仓库地址
```

结果：

```text
本地仓库知道应该把代码推送到哪个 GitHub 仓库。
```

如果出现：

```text
remote origin already exists
```

说明已经有 origin，不要重复添加。

可以查看：

```bash
git remote -v
```

如需修改地址：

```bash
git remote set-url origin 新地址
```

### 14.3 `git push -u origin main`

作用：

```text
第一次把本地 main 分支上传到远程 origin，并建立追踪关系。
```

命令：

```bash
git push -u origin main
```

逐词解释：

```text
git     调用 Git
push    上传提交
-u      upstream，建立本地分支和远程分支的追踪关系
origin  远程仓库别名
main    要上传的本地分支
```

典型结果：

```text
* [new branch] main -> main
branch 'main' set up to track 'origin/main'.
```

含义：

```text
GitHub 上创建/更新了 main 分支
本地 main 已经和 origin/main 建立关联
以后可以直接 git push
```

### 14.4 `git push`

作用：

```text
把本地新提交上传到远程仓库。
```

命令：

```bash
git push
```

逐词解释：

```text
git   调用 Git
push  推送，上传本地提交
```

典型结果：

```text
To https://github.com/ChenxinRuyi2004/Git-Learning.git
   00a5915..625c804  main -> main
```

含义：

```text
远程 main 从 00a5915 更新到了 625c804
本地提交已经上传到 GitHub
```

### 14.5 `git pull`

作用：

```text
从远程仓库拉取更新，并合并到当前分支。
```

命令：

```bash
git pull
```

逐词解释：

```text
git   调用 Git
pull  拉取远程更新并合并
```

可以理解为：

```text
git pull = git fetch + git merge
```

典型结果：

```text
Updating 625c804..xxxxxxx
Fast-forward
 github-pull-practice.txt | 1 +
 create mode 100644 github-pull-practice.txt
```

含义：

```text
GitHub 上的新提交已经拉到本地
本地新增了 github-pull-practice.txt
```

### 14.6 `git fetch`

作用：

```text
只下载远程最新历史，不立刻合并，不改当前工作区文件。
```

命令：

```bash
git fetch
```

逐词解释：

```text
git    调用 Git
fetch  获取远程更新
```

区别：

```text
fetch = 先看看远程有什么新东西
pull  = 下载远程新东西并合并到当前分支
```

常见查看：

```bash
git log --oneline --graph --decorate --all
```

如果看到：

```text
origin/main 在 main 前面
```

说明：

```text
远程 main 比本地 main 更新。
```

可以手动合并：

```bash
git merge origin/main
```

## 15. push 失败怎么办

### 15.1 常见失败原因

如果远程比本地更新，本地直接 push 可能失败。

常见关键词：

```text
rejected
non-fast-forward
fetch first
Updates were rejected
```

含义：

```text
GitHub 上有你本地没有的提交
你不能直接覆盖远程
需要先拉取远程更新
```

### 15.2 解决公式

```bash
git pull
git push
```

如果 pull 出现冲突：

```text
1. 打开冲突文件
2. 手动改成最终内容
3. 删除冲突标记
4. git add 冲突文件
5. git commit
6. git push
```

### 15.3 pull 提示选择策略

如果看到：

```text
Need to specify how to reconcile divergent branches
```

新手可以设置：

```bash
git config --global pull.rebase false
```

含义：

```text
以后 git pull 默认用 merge 方式处理分叉。
```

## 16. 文件删除

### 16.1 手动删除后提交删除

如果你手动删除了一个被 Git 管理的文件，`git status` 会显示：

```text
deleted: 文件名
```

如果确认要删除：

```bash
git add 文件名
git commit -m "删除文件名"
git push
```

这里 `git add 文件名` 的含义是：

```text
把“这个文件被删除了”这件事加入暂存区。
```

### 16.2 使用 `git rm`

如果文件还在，但你想让 Git 删除它：

```bash
git rm 文件名
git commit -m "删除文件名"
git push
```

逐词解释：

```text
git   调用 Git
rm    remove，删除文件，并暂存删除动作
文件名 要删除的文件
```

`git rm 文件名` 等于：

```text
删除本地文件 + git add 删除动作
```

### 16.3 恢复误删文件

如果误删了：

```bash
git restore 文件名
```

含义：

```text
从当前最新提交中把这个文件恢复回来。
```

## 17. GitHub 文件类型

GitHub 不会自动给文件加后缀。

如果你创建：

```text
github-note
```

它就是没有扩展名的文件。

如果你创建：

```text
github-note.txt
```

它才是 `.txt` 文件。

常见文件都可以放：

```text
.txt
.md
.py
.js
.html
.css
.json
.csv
.pdf
.doc
.docx
.ppt
.pptx
.xlsx
.png
.jpg
.zip
```

但 Git 最擅长管理文本文件：

```text
.txt
.md
代码文件
配置文件
```

对于这些文件，`git diff` 能看清每一行改了什么。

对于这些文件：

```text
.pdf
.docx
.pptx
.xlsx
```

Git 可以保存和上传，但通常不能很好显示里面具体改了哪句话或哪一页。

## 18. Pull Request 和 Fork

### 18.1 Pull Request 是什么

Pull Request 简称 PR，意思是：

```text
我在一个分支上改好了内容，请求把它合并到主分支。
```

它不是 Git 命令，而是 GitHub/Gitee 网站上的协作流程。

Gitee 上常叫 Merge Request，简称 MR。

### 18.2 自己的仓库和别人的仓库

自己的仓库：

```text
通常可以直接 git push
```

别人的仓库：

```text
默认不能直接 git push
除非对方给你写入权限
```

没有权限时常见错误：

```text
Permission denied
403
repository not found
```

### 18.3 贡献别人项目的标准流程

假设原仓库是：

```text
https://github.com/original-owner/project
```

你的账号是：

```text
ChenxinRuyi2004
```

流程：

```text
1. Fork 别人的仓库到你自己账号
2. Clone 你 fork 后的仓库
3. 添加原仓库为 upstream
4. 新建分支修改
5. Push 到你自己的 fork
6. 向原仓库发 Pull Request
7. 等维护者同意合并
```

### 18.4 Fork

Fork 是：

```text
在 GitHub 网站上，把别人的仓库复制一份到你的账号下面。
```

原仓库：

```text
https://github.com/original-owner/project
```

你的 fork：

```text
https://github.com/ChenxinRuyi2004/project
```

### 18.5 Clone 你的 fork

```bash
git clone https://github.com/ChenxinRuyi2004/project.git
cd project
```

这时：

```text
origin 指向你的 fork
```

### 18.6 添加 upstream

```bash
git remote add upstream https://github.com/original-owner/project.git
```

逐词解释：

```text
git       调用 Git
remote    管理远程仓库
add       添加
upstream  原仓库常用别名
https://... 原仓库地址
```

检查：

```bash
git remote -v
```

应看到：

```text
origin    你的 fork
upstream  原作者仓库
```

记忆：

```text
origin = 你自己的 GitHub 副本
upstream = 原作者的仓库
```

### 18.7 新建分支并提交

```bash
git switch -c fix-typo
# 修改文件
git add .
git commit -m "Fix typo in README"
```

### 18.8 Push 到你的 fork

```bash
git push -u origin fix-typo
```

含义：

```text
把 fix-typo 分支上传到你的 fork。
```

注意：

```text
这里 push 到 origin，不是 upstream。
因为你通常没有 upstream 原仓库的写入权限。
```

### 18.9 创建 PR

在 GitHub 网页确认方向：

```text
base repository: original-owner/project
base branch: main

head repository: ChenxinRuyi2004/project
compare branch: fix-typo
```

含义：

```text
请求把你 fork 里的 fix-typo 分支
合并到原仓库的 main 分支
```

如果维护者要求修改：

```bash
# 继续在 fix-typo 分支修改
git add .
git commit -m "Address review comments"
git push
```

PR 会自动更新。

### 18.10 同步原仓库更新

```bash
git switch main
git fetch upstream
git merge upstream/main
git push origin main
```

含义：

```text
从原仓库下载更新
合并到本地 main
再推到你的 fork
```

## 19. 常见命令错误

### 19.1 `gitdiff`

错误：

```bash
gitdiff
```

结果：

```text
bash: gitdiff: command not found
```

原因：

```text
git 和 diff 之间少了空格。
```

正确：

```bash
git diff
```

### 19.2 `git sattus`

错误：

```bash
git sattus
```

结果：

```text
git: 'sattus' is not a git command.
The most similar command is
        status
```

原因：

```text
status 拼错了。
```

正确：

```bash
git status
```

### 19.3 `git log --online`

错误：

```bash
git log --online --graph
```

原因：

```text
Git 参数是 --oneline，不是 --online。
```

正确：

```bash
git log --oneline --graph
```

### 19.4 中文引号导致 `>` 续行

错误：

```bash
git commit -m "修改文件“
```

结果：

```text
>
```

处理：

```text
按 Ctrl + C
重新输入英文引号
```

正确：

```bash
git commit -m "修改文件"
```

### 19.5 LF/CRLF warning

可能看到：

```text
warning: in the working copy of 'demo/hello.txt',
LF will be replaced by CRLF the next time Git touches it
```

含义：

```text
这是 Windows 换行符提示。
LF 是 Linux/macOS 常见换行
CRLF 是 Windows 常见换行
```

新手阶段：

```text
通常不用紧张，不影响 Git 主流程学习。
```

## 20. 当前项目中出现过的提交历史

可以用：

```bash
git log --oneline --graph --decorate --all
```

看到类似：

```text
* 5cf2d51 (HEAD -> main, origin/main, origin/HEAD) “删除我的自述”
* 625c804 练习，本地创建文件并推送
* 00a5915 Add self-description stating personal confidence
* 71a8da3 练习，在分支上修改hello
* 61932f5 提交第二部分
* 98497de 只提交一部分
* 4bc7da9 第二次练习
* 3876e0d 修改文件
* 3c00927 初始化 Git 学习仓库
```

读法：

```text
最上面是最新提交
HEAD -> main 表示当前本地 main 指向这里
origin/main 表示 GitHub 的 main 也指向这里
origin/HEAD 表示远程默认分支也在这里
```

## 21. 日常个人项目标准流程

每次开始：

```bash
git pull
```

修改文件后：

```bash
git status
git diff
git add .
git diff --staged
git commit -m "说明这次做了什么"
git push
```

简化记忆：

```text
先 pull
再修改
看 status
看 diff
add 暂存
commit 存本地
push 上传
```

## 22. 团队项目标准流程

```bash
git pull
git switch -c feature/my-work
# 修改文件
git status
git add .
git commit -m "完成某个功能"
git push -u origin feature/my-work
```

然后去 GitHub 创建 Pull Request。

合并后：

```bash
git switch main
git pull
git branch -d feature/my-work
```

## 23. 常用指令集合

### 23.1 初始化和克隆

```bash
git init
git clone 仓库地址
```

### 23.2 配置

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
git config --global core.quotepath false
```

### 23.3 查看状态和历史

```bash
git status
git log
git log --oneline
git log --oneline --graph
git log --oneline --graph --decorate --all
git ls-files
```

### 23.4 查看差异

```bash
git diff
git diff --staged
git diff HEAD
git show 提交ID
```

### 23.5 暂存和提交

```bash
git add 文件名
git add .
git commit -m "提交说明"
```

### 23.6 撤销

```bash
git restore 文件名
git restore --staged 文件名
git restore --source=提交ID 文件名
git revert 提交ID
```

### 23.7 分支

```bash
git branch
git switch -c 分支名
git switch 分支名
git merge 分支名
git branch -d 分支名
```

### 23.8 远程

```bash
git remote -v
git remote add origin 仓库地址
git remote set-url origin 新地址
git push -u origin main
git push
git pull
git fetch
```

### 23.9 删除

```bash
git rm 文件名
git rm --cached 文件名
git add 已经被手动删除的文件名
git commit -m "删除文件"
```

### 23.10 Fork/PR 协作

```bash
git clone 你的fork地址
git remote add upstream 原仓库地址
git switch -c 分支名
git add .
git commit -m "提交说明"
git push -u origin 分支名
git fetch upstream
git merge upstream/main
```

## 24. 最应该背下来的 12 个命令

```bash
git status
git diff
git add .
git commit -m "提交说明"
git log --oneline --graph
git restore 文件名
git restore --staged 文件名
git branch
git switch -c 分支名
git merge 分支名
git pull
git push
```

## 25. 最重要的心法

### 25.1 先看状态，再做操作

任何时候不确定，就先：

```bash
git status
```

### 25.2 提交前看差异

```bash
git diff
git diff --staged
```

这样可以避免把错误内容提交进去。

### 25.3 commit 是存档点

每次 commit 都是一个可以查找、对比、恢复的版本。

```text
工作区是现在
暂存区是准备存档的内容
本地仓库历史是过去所有存档
远程仓库是网上同步的存档
```

### 25.4 push 前先保证自己理解要上传什么

常用：

```bash
git status
git log --oneline --graph --decorate --all
```

### 25.5 看到错误先读关键词

```text
not staged       改了但没 add
to be committed  已 add，准备 commit
untracked        新文件，Git 还没追踪
deleted          文件被删除
conflict         冲突，需要手动解决
rejected         push 被拒绝，通常先 pull
```

## 26. 一句话总复习

```text
Git 是本地版本管理工具。
GitHub/Gitee 是远程托管平台。
工作区负责修改，暂存区负责准备，本地仓库负责保存历史，远程仓库负责同步协作。
日常就是 pull、改文件、status、diff、add、commit、push。
遇到分支就 switch、merge。
遇到冲突就手动改文件、add、commit。
遇到别人的项目就 fork、clone、push 到自己的 fork、发 PR。
```
