# Lazygit 高效使用手册 (Lazygit Usage Guide)

Lazygit 是一个简单但功能强大的 Git 终端 UI 客户端，使用 Go 语言编写，旨在提高 Git 工作流的效率。

## 1. 界面概览 (The Layout)
Lazygit 的界面分为 5 个主要面板（按 `1-5` 数字键切换）：
1. **Status**: 仓库状态及配置。
2. **Files**: 暂存与工作区文件（最常用）。
3. **Branches**: 本地与远程分支。
4. **Commits**: 提交历史。
5. **Stash**: 贮藏列表。

中间大区域显示 **Diff/Log**，最下方是 **Command Log**。

## 2. 核心操作 (Core Operations)
### 2.1 文件操作 (Files Panel - `2`)
- `Space` - 切换文件的暂存状态 (Stage/Unstage)
- `a` - 暂存/取消暂存所有文件 (Stage/Unstage all)
- `c` - 提交更改 (Commit)
- `C` - 使用编辑器提交 (Commit using external editor)
- `d` - 丢弃文件更改 (Discard changes)
- `e` - 编辑文件 (Edit file)
- `i` - 切换忽略文件 (Add to .gitignore)

### 2.2 分支管理 (Branches Panel - `3`)
- `Space` - 切换到选中的分支 (Checkout)
- `n` - 创建新分支 (New branch)
- `d` - 删除分支 (Delete branch)
- `M` - 合并分支 (Merge)
- `r` - 变基到该分支 (Rebase)
- `f` - 强制推送 (Force push)

### 2.3 提交历史 (Commits Panel - `4`)
- `Enter` - 查看该提交中的具体文件更改
- `s` - 压缩提交 (Squash down)
- `r` - 重新编写提交消息 (Reword commit)
- `R` - 硬重置到该提交 (Hard reset)
- `v` - 还原提交 (Revert commit)
- `A` - 修正最后一次提交 (Amend commit)

## 3. 进阶技巧 (Advanced Features)
### 3.1 交互式变基 (Interactive Rebase)
在 `Commits` 面板中，按下 `i` 开启交互式变基：
- `pick` / `skip` / `squash` / `fixup`
- 配合键盘上下移动提交顺序。

### 3.2 樱桃摘取 (Cherry-picking)
1. 在 `Commits` 面板选中目标提交，按 `c` (Copy)。
2. 切换到目标分支。
3. 按 `v` (Paste) 将提交摘取过来。

### 3.3 解决冲突 (Conflict Resolution)
当发生冲突时，Lazygit 会自动进入冲突解决模式：
- `Space` - 选择当前行
- `b` - 同时保留两边 (Both)
- `n` / `p` - 跳转到 下一个/上一个 冲突点

## 4. 常用快捷键速查
| 键位 | 功能 |
| :--- | :--- |
| `q` | 退出 Lazygit |
| `?` | 打开帮助菜单（最重要的键！） |
| `p` | 拉取 (Pull) |
| `P` | 推送 (Push) |
| `x` | 打开操作菜单 (Menu) |
| `z` | 撤销上一个 Lazygit 操作 (Undo) |
| `Ctrl + r` | 刷新状态 |

## 5. 配置建议 (config.yml)
Lazygit 的配置文件通常位于 `~/.config/lazygit/config.yml`。
建议开启“确认退出”以防误操作：
```yaml
gui:
  confirmOnQuit: true
  showIcons: true # 如果安装了 Nerd Fonts
git:
  paging:
    colorArg: always
    pager: delta --dark --paging=never # 配合 delta 获得更好的 Diff 体验
```

## 6. 与 Vim 集成
如果你是 Vim/Neovim 用户，推荐安装 `lazygit.nvim` 插件，实现在编辑器内通过快捷键一键唤起 Lazygit。

---
*文档生成日期: 2026-03-30*
*提示：在任何面板按下 `x` 都可以看到该面板支持的所有操作。*
