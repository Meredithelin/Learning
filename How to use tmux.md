# Tmux 终极使用指南：从入门到神级配置

`tmux` (Terminal Multiplexer) 是开发者的终端神器。它的核心价值在于：**分屏**、**会话保护**（断网或关闭终端后任务不中断）以及**极高的可定制性**。

---

## 零、 核心概念与前缀键 (Prefix)

`tmux` 的层级关系：**Server (服务) -> Session (会话) -> Window (窗口) -> Pane (面板)**。

**默认前缀键**：`Ctrl + b` (下文简写为 `Prefix`)。
**操作逻辑**：先按 `Ctrl + b`，**松开**，再按功能键。

---

## 一、 基础操作：会话、窗口与面板

### 1. 会话管理 (Session) —— 保护工作现场
*在 Linux 终端（非 tmux 内部）运行：*
- **新建并命名**：`tmux new -s <name>` (如 `tmux new -s dev`)
- **查看所有会话**：`tmux ls`
- **连回最近会话**：`tmux a`
- **连回指定会话**：`tmux a -t <name>`
- **断开当前会话 (挂起)**：`Prefix` + `d` (Detach)
- **销毁指定会话**：`tmux kill-session -t <name>`

### 2. 窗口管理 (Window) —— 类似浏览器标签页
- **新建窗口**：`Prefix` + `c`
- **切换窗口**：`Prefix` + `n` (后一个) / `p` (前一个) / `数字` (指定编号)
- **列出窗口并切换**：`Prefix` + `w` (强烈推荐，支持上下键预览)
- **重命名当前窗口**：`Prefix` + `,`

### 3. 面板管理 (Pane) —— 强大的分屏
- **左右分屏**：`Prefix` + `%`
- **上下分屏**：`Prefix` + `"`
- **切换光标**：`Prefix` + `方向键` 或 `Prefix` + `q` (显示数字编号并按数字跳转)
- **全屏/取消全屏**：`Prefix` + `z` (Zoom，查看日志神器)
- **调整面板大小**：按住 `Prefix` 的同时，按 `方向键` 微调边界

---

## 二、 进阶配置：让 Tmux 更好用

### 1. 打通 WSL 与 Windows 剪贴板
在 `~/.tmux.conf` 中添加以下配置，配合 `clip.exe` 实现无缝复制。

```tmux
# 开启 vi 模式进行全键盘复制
setw -g mode-keys vi
bind -T copy-mode-vi v send-keys -X begin-selection
# 复制到 Windows 剪贴板
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "clip.exe"
bind -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "clip.exe"
# 鼠标拖拽自动复制
bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "clip.exe"
```

### 2. 鼠标与外观优化
```tmux
set -g mouse on               # 开启鼠标支持（点击切换、拖拽调整大小、滚轮翻页）
set-option -g allow-rename off # 禁止程序自动修改你的窗口命名
set -g pane-border-status top  # 在分屏顶部显示标题栏
```

---

## 三、 插件系统：开启“神级”体验 (TPM)

要安装插件，首先需要安装 **Tmux Plugin Manager (TPM)**：
`git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

### 1. 核心插件推荐
在 `~/.tmux.conf` 底部添加：

```tmux
# 插件列表
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'christoomey/vim-tmux-navigator' # Vim 与 Tmux 无缝导航
set -g @plugin 'tmux-plugins/tmux-resurrect'     # 重启后恢复会话
set -g @plugin 'tmux-plugins/tmux-continuum'     # 自动保存会话

# 插件配置
set -g @continuum-restore 'on' # 启动 tmux 时自动恢复上一次保存的会话

# 初始化 TPM (这行必须放在文件最底部)
run '~/.tmux/plugins/tpm/tpm'
```

### 2. 插件快捷键
- **安装插件**：`Prefix` + `I` (大写 I，Install)
- **更新插件**：`Prefix` + `u`
- **手动保存状态**：`Prefix` + `Ctrl + s`
- **手动恢复状态**：`Prefix` + `Ctrl + r`

---

## 四、 完整配置示例 (`~/.tmux.conf`)

如果你想一步到位，可以将以下内容覆盖到你的配置文件中：

```tmux
# --- 基础设置 ---
set -g default-terminal "screen-256color"
set -g mouse on
set-option -g allow-rename off

# --- 快捷键解绑与重绑 ---
# 绑定 Prefix + r 快速重载配置
bind r source-file ~/.tmux.conf \; display "Config Reloaded!"

# --- 分屏标题与重命名 ---
set -g pane-border-status top
set -g pane-border-format " [ #[fg=cyan,bold]#P #[default]] #[fg=blue,bold]#T #[default]"
bind T command-prompt -p "Pane Title:" -I "#T" "select-pane -T '%%'"

# --- 剪贴板 (WSL 专用) ---
setw -g mode-keys vi
bind -T copy-mode-vi v send-keys -X begin-selection
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "clip.exe"

# --- 插件管理 (TPM) ---
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'christoomey/vim-tmux-navigator'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

set -g @continuum-restore 'on'

run '~/.tmux/plugins/tpm/tpm'
```

---

## 五、 Tmux 快捷键速查表

| 类别 | 快捷键 (Prefix + ...) | 描述 |
| :--- | :--- | :--- |
| **基础** | `r` | 重载配置文件 (需自定义绑定) |
| **窗口** | `c` | 新建窗口 |
| **窗口** | `,` | 重命名当前窗口 |
| **窗口** | `w` | 列出所有窗口并切换 |
| **分屏** | `%` / `"` | 左右 / 上下分屏 |
| **分屏** | `z` | 最大化/还原当前面板 |
| **分屏** | `x` | 关闭当前面板 |
| **导航** | `q` | 显示面板编号（输入数字跳转） |
| **导航** | `Ctrl + h/j/k/l` | 无缝穿梭 (需安装 navigator 插件) |
| **模式** | `[` | 进入复制/翻页模式 (`q` 退出) |
| **插件** | `I` | (大写) 安装插件 |
| **插件** | `Ctrl + s / r` | 保存 / 恢复会话状态 |
