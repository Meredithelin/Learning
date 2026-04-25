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
### 3.1 分支基本操作 (Detailed git branch)

**查看分支**
- `git branch`：查看本地所有分支，当前分支前有 `*` 标记。
- `git branch -r`：只查看**远程追踪**分支（如 `origin/main`）。
- `git branch -a`：查看所有分支（本地 + 远程追踪分支）。
- `git branch -v`：查看每个分支的最后一次提交（commit ID + 提交信息）。
- `git branch -vv`：在 `-v` 基础上，还显示每个本地分支正在**跟踪哪个远程分支**，以及领先/落后多少次提交。

**创建与切换分支**
- `git branch <name>`：基于当前提交创建新分支，但不切换。
- `git branch <name> <commit>`：基于指定提交或标签创建分支。
- `git switch -c <name>`：**推荐**。创建并切换到新分支（替代旧的 `git checkout -b`）。
- `git switch -c <name> origin/<name>`：创建本地分支并自动追踪同名远程分支。

**删除与重命名**
- `git branch -d <name>`：删除**已合并**的分支（安全删除）。
- `git branch -D <name>`：**强制**删除未合并的分支（数据可能丢失）。
- `git branch -m <old> <new>`：重命名分支。
- `git push origin --delete <name>`：删除**远程**分支。

**追踪关系（Tracking）**
- `git branch --set-upstream-to=origin/<remote_branch> <local_branch>`：为已有本地分支设置追踪的远程分支。例如：
  ```bash
  git branch --set-upstream-to=origin/main main
  ```
- 设置追踪后，在该分支上直接执行 `git push` / `git pull` 即可，无需再指定远程和分支名。

**筛选合并状态**
- `git branch --merged`：查看哪些分支已经**合并**到当前分支（可安全删除）。
- `git branch --no-merged`：查看哪些分支**尚未**合并。

### 3.2 合并与冲突解决
- `git merge <name>`：合并指定分支到当前分支。
- `git merge --squash <name>`：压缩合并。将分支上的多次提交合并为一次提交再合入。
- `git cherry-pick <commit_id>`：挑拣提交。将另一个分支上的特定提交应用到当前分支。

**冲突解决 (Conflict Resolution)**：
1. **定位冲突**：Git 会在文件中插入冲突标记 `<<<<<<<`, `=======`, `>>>>>>>`。
2. **人工抉择**：编辑文件，保留想要的代码，删除 Git 插入的标记。
3. **完成合并**：执行 `git add <file>` 标记冲突已解决，然后 `git commit`。
4. **中止合并**：若想撤销合并过程，执行 `git merge --abort`。

### 3.3 差异比较 (Detailed git diff)

**比较工作区 / 暂存区 / 提交**
- `git diff`：比较**工作区** (Working Directory) 与**暂存区** (Index) 的差异（尚未 `git add` 的变化）。
- `git diff --cached` 或 `git diff --staged`：比较**暂存区**与**最后一次提交** (HEAD) 的差异（已 `add` 但未 `commit` 的变化）。
- `git diff HEAD`：比较**工作区**与**最后一次提交**的所有差异（包含已暂存和未暂存）。

**指定范围比较**
- `git diff <commit1> <commit2>`：比较两次提交之间的差异。
- `git diff <branch1> <branch2>`：查看两个分支最新提交的完整差异（双点，比较两个端点）。
- `git diff <branch1>...<branch2>`：**三点语法**。找到两个分支的**共同祖先**，然后比较 `<branch2>` 相对于该祖先引入了哪些变化（即"这个分支独有的改动"）。用于 Code Review 时非常实用。
- `git diff HEAD~1 HEAD`：查看最近一次提交引入的改动（等价于 `git show HEAD`）。

**只看特定文件**
- `git diff <file>`：只查看某个具体文件的工作区差异。
- `git diff <commit1> <commit2> -- <file>`：只查看某个文件在两次提交之间的差异。

**控制输出格式**
- `git diff --stat`：只显示**统计摘要**（修改了哪些文件，增删了多少行），不显示具体代码。
- `git diff --name-only`：只列出**发生变化的文件名**，不显示任何内容。
- `git diff --word-diff`：以**单词**为单位显示差异，而非以行为单位，对改动较小的文本非常清晰。

### 3.4 理解 HEAD~1 与相对引用

在 Git 中，`HEAD` 是一个指针，指向**当前所在的提交**。你可以用 `~` 和 `^` 从它出发向历史回溯，无需记忆 commit ID。

**`~`（波浪号）：沿第一父节点回溯**
- `HEAD` 或 `HEAD~0`：当前提交本身。
- `HEAD~1`（或简写 `HEAD~`）：当前提交的**父提交**（上一个提交）。
- `HEAD~2`：父提交的父提交（往前数两步）。
- `HEAD~n`：往前数 n 步的提交，始终沿**第一父节点**（first-parent）路径。

**`^`（脱字符）：选择第几个父节点**
- `HEAD^`：等同于 `HEAD~1`，即父提交。
- `HEAD^2`：在**合并提交**（merge commit）中，选择**第二个父节点**，即被合并进来的那个分支的最新提交。
- `HEAD~1^2`：往前一步，再取第二个父节点。

**图示示例**：
```
A --- B --- C --- D (HEAD, main)
              \
               E --- F (feature, 已被合并入 D)
```
- `HEAD~1` = `C`（D 的第一父节点）
- `HEAD^2` = `F`（D 是合并提交，^2 指向被合并来的分支）

**实际应用**：
- `git diff HEAD~1`：查看最近一次提交改动了什么。
- `git reset --soft HEAD~1`：撤销最近一次提交，保留代码在暂存区。
- `git show HEAD~2`：查看倒数第三个提交的内容。

---

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
- `git fetch`：**同步远程状态**（只下载，不修改本地分支）。
    - 它会**下载**远程仓库的所有新提交、分支和标签到本地。
    - 它会更新"远程追踪分支"（如 `origin/main`），但**不会**修改你的任何本地分支（如 `main`）。
    - 这意味着你可以安全地"看一眼"远程有什么新内容，完全不影响本地的工作。
    - `git fetch origin`：拉取名为 `origin` 的远程仓库的所有更新。
    - `git fetch origin <branch>`：只拉取远程特定分支的更新。
    - `git fetch --all`：拉取**所有**已配置的远程仓库的更新。
    - **fetch 后的典型工作流**：
      ```bash
      git fetch origin          # 1. 下载远程更新，不影响本地
      git diff main origin/main # 2. 对比本地和远程的差异
      git merge origin/main     # 3. 确认无误后，将远程变更合并到本地（或用 rebase）
      ```
- `git pull --rebase`：**进阶推荐**。相当于 `git fetch` + `git rebase`。拉取并变基，保持提交历史线性整洁。
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
