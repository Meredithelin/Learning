# Git 使用手册 (Git Usage Guide)

Git 是一个分布式版本控制系统，旨在快速高效地处理从小到大所有项目的版本管理。

## 1. 基础配置
在开始之前，请设置你的身份标识：
```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
# 设置默认分支名为 main
git config --global init.defaultBranch main
```

## 2. 基础工作流
### 2.1 初始化与克隆
- `git init` - 在当前目录初始化一个新的仓库
- `git clone <url>` - 克隆远程仓库到本地

### 2.2 暂存与提交
- `git status` - 查看当前工作区和暂存区的状态
- `git add <file>` - 将文件添加到暂存区
- `git add .` - 将所有更改添加到暂存区
- `git commit -m "提交消息"` - 提交暂存区的更改
- `git commit --amend` - 修改上一次提交（如果尚未 push）

### 2.3 查看历史
- `git log` - 查看详细的提交历史
- `git log --oneline --graph --all` - 以图形化方式查看简略历史
- `git diff` - 查看工作区与暂存区的差异

## 3. 分支管理 (Branching)
- `git branch` - 列出本地分支
- `git branch <name>` - 创建新分支
- `git checkout <name>` - 切换到指定分支
- `git checkout -b <name>` - 创建并切换到新分支
- `git merge <name>` - 将指定分支合并到当前分支
- `git branch -d <name>` - 删除已合并的分支
- `git branch -D <name>` - 强制删除分支

## 4. 远程操作 (Remote)
- `git remote -v` - 查看关联的远程仓库地址
- `git remote add origin <url>` - 关联远程仓库
- `git push origin <branch>` - 推送本地提交到远程
- `git pull origin <branch>` - 拉取远程更新并合并
- `git fetch` - 仅拉取远程更新，不自动合并

## 5. 撤销与恢复 (Undo)
- `git restore <file>` - 丢弃工作区的更改
- `git restore --staged <file>` - 将文件从暂存区移回工作区
- `git reset --soft HEAD~1` - 撤销最近一次提交，保留更改在暂存区
- `git reset --hard HEAD~1` - **彻底丢弃**最近一次提交及更改（慎用！）
- `git revert <commit_id>` - 创建一个反向提交来抵消指定提交的影响

## 6. 进阶技巧
### 6.1 贮藏 (Stash)
临时保存未提交的工作，以便切换分支。
- `git stash` - 保存当前工作进度
- `git stash pop` - 恢复并移除最近一次贮藏
- `git stash list` - 查看所有贮藏

### 6.2 变基 (Rebase)
保持提交历史的线性美观。
- `git rebase <branch>` - 将当前分支基点移至目标分支最新处
- `git rebase -i HEAD~3` - 交互式变基，用于合并或修改历史提交

## 7. 最佳实践
- **频繁提交，小步快跑**：每次提交只解决一个问题。
- **编写有意义的提交消息**：采用 `feat: ...`, `fix: ...` 等规范。
- **先拉取再推送**：在 `push` 之前先 `pull --rebase` 保持历史整洁。
- **不要对已推送到远程的历史执行 rebase**：这会破坏协作。

---
*文档生成日期: 2026-03-21*
