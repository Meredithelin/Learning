# Git 深度学习笔记

这份笔记旨在从基础到进阶掌握 Git 的核心用法，涵盖日常开发中的各种场景。

## 0. 环境配置 (Config)
在开始之前，确保你的身份已配置，这会出现在每一条提交记录中。

- `git config --global user.name "Your Name"`：设置用户名。
- `git config --global user.email "email@example.com"`：设置邮箱。
- `git config --global alias.st status`：**配置别名**。比如设置后 `git st` 就等于 `git status`。
- `git config --list`：查看当前所有配置。

## 1. 基础工作流：从修改到提交
- `git status`：**核心命令**。随时查看工作区和暂存区的状态。
- `git add <file>`：将修改加入暂存区（Index）。
- `git add .`：将当前目录下所有修改加入暂存区。
- `git commit -m "msg"`：记录一个新的快照。
- `git log`：查看提交历史；`--oneline --graph` 可以查看图形化简要历史。
- `git show <commit_id>`：查看某次提交的具体变动内容。
- `git diff`：查看工作区与暂存区的差异；`git diff --cached` 查看暂存区与最后一次提交的差异。

## 2. 文件操作技巧
避免直接用系统命令 `rm` 或 `mv`，使用 Git 命令能自动处理暂存区变更。

- `git mv <old> <new>`：移动或重命名文件。
- `git rm <file>`：从工作区和索引中删除文件。

## 3. 分支管理与冲突解决
### 3.1 分支基本操作
- `git branch`：查看本地分支；`-a` 查看所有分支（含远程）。
- `git switch -c <name>`：**推荐**。创建并切换到新分支（替代旧的 `git checkout -b`）。
- `git merge <name>`：合并指定分支到当前分支。
- `git merge --squash <name>`：压缩合并。将分支上的多次提交合并为一次提交再合入。
- `git cherry-pick <commit_id>`：挑拣提交。将另一个分支上的特定提交应用到当前分支。

### 3.2 冲突解决 (Conflict Resolution)
当两个分支修改了同一个文件的同一行时，合并会产生冲突。
1. **定位冲突**：Git 会在文件中插入冲突标记 `<<<<<<<`, `=======`, `>>>>>>>`。
2. **人工抉择**：编辑文件，保留想要的代码，删除 Git 插入的标记。
3. **完成合并**：执行 `git add <file>` 标记冲突已解决，然后 `git commit`。
4. **中止合并**：若想撤销合并过程，执行 `git merge --abort`。

## 4. 撤销与“后悔药”
### 4.1 修改最后一次提交
- `git commit --amend`：漏了文件或注释写错了。

### 4.2 撤销修改
- `git restore <file>`：丢弃工作区的修改（回到最后一次提交的状态）。
- `git restore --staged <file>`：将文件从暂存区撤回到工作区（unstage）。

### 4.3 重置历史 (Reset)
- `git reset --soft HEAD~1`：**保留代码**。回退版本，所有代码变更都留在暂存区。
- `git reset --mixed HEAD~1`：**默认模式**。回退版本，代码变更留在工作区（未暂存）。
- `git reset --hard HEAD~1`：**慎用**。彻底回退到某个版本，丢失所有未提交修改。

### 4.4 安全回滚 (Revert)
- `git revert <commit_id>`：通过产生一个新的提交来抵消指定提交的变化。**适用于已推送远程的分支。**

## 5. 远程操作与配置
### 5.1 远程仓库配置
- `git remote -v`：查看远程库详细信息。
- `git remote add origin <url>`：关联远程仓库。
- `git remote set-url origin <newurl>`：修改远程仓库地址。

### 5.2 协作流程
- `git fetch`：拉取远程仓库的所有变动，但不自动合并。
- `git pull --rebase`：**进阶推荐**。拉取并变基，保持提交历史线性整洁。
- `git push origin <branch>`：推送本地提交到远程分支。

## 6. 进阶：贮藏与标签
- `git stash`：暂时保存当前未完成的工作（工作区变干净）。
- `git stash pop`：恢复并删除最近一次贮藏。
- `git tag v1.0`：在当前提交上打一个标签。
- `git tag -a v1.0 -m "release version"`：创建一个带说明的标签。
- `git push origin v1.0`：推送本地标签到远程。

## 7. 核心场景速查 (Cheat Sheet)

| 场景 | 推荐命令 |
| :--- | :--- |
| 想看看我改了啥还没 add | `git diff` |
| 提交后发现少加了一个文件 | `git add <file> && git commit --amend` |
| 写乱了，想彻底回到上次提交 | `git reset --hard HEAD` |
| 误删了分支想找回 | `git reflog` (查找 commit ID) |
| 远程有更新，我想让我的提交在它们之后 | `git pull --rebase` |

---
*更新于: 2026-03-23*
