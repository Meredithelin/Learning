# Zellij 使用指南

`zellij` 是一款用 Rust 编写的现代终端多路复用器（Terminal Multiplexer），功能类似 tmux，但提供了更直观的模式切换界面、内置布局系统和插件支持。

---

## 1. 安装

```bash
# macOS (Homebrew)
brew install zellij

# Arch Linux
pacman -S zellij

# Cargo (通用)
cargo install --locked zellij

# 下载预编译二进制
# 前往 https://github.com/zellij-org/zellij/releases 下载对应平台的版本
```

---

## 2. 核心概念

Zellij 采用**模式 (Mode)** 驱动的操作方式，类似 Vim。每种模式下的快捷键相互独立，避免冲突。

| 概念 | 说明 |
| :--- | :--- |
| **Session（会话）** | 最顶层容器，可以后台运行并随时重新连接 |
| **Tab（标签页）** | 会话中的逻辑分组，类似浏览器标签页 |
| **Pane（面板）** | 标签页内的分割终端窗口 |
| **Mode（模式）** | 当前键盘的操作上下文，底部状态栏实时显示 |

---

## 3. 启动与会话管理

```bash
zellij                          # 启动新会话
zellij -s <name>                # 启动并命名会话
zellij attach <name>            # 连接到已有会话
zellij attach --create <name>   # 连接或创建会话
zellij list-sessions            # 列出所有会话
zellij kill-session <name>      # 关闭指定会话
zellij kill-all-sessions        # 关闭所有会话
```

---

## 4. 模式切换与核心快捷键

> **操作逻辑**：按下模式对应的组合键 → 进入该模式 → 执行操作 → 按 `Esc` 或 `Enter` 返回 Normal 模式。
> 底部状态栏会高亮显示当前可用的快捷键。

### Normal 模式（默认）

直接在终端中输入，所有输入传递给 Shell。

| 快捷键 | 进入的模式 |
| :--- | :--- |
| `Ctrl + p` | **Pane 模式**（面板管理） |
| `Ctrl + t` | **Tab 模式**（标签页管理） |
| `Ctrl + n` | **Resize 模式**（调整面板大小） |
| `Ctrl + h` | **Move 模式**（移动面板位置） |
| `Ctrl + s` | **Scroll 模式**（滚动/搜索历史） |
| `Ctrl + o` | **Session 模式**（会话管理） |
| `Ctrl + b` | **Tmux 兼容模式** |
| `Ctrl + q` | 退出 Zellij |

---

### Pane 模式（`Ctrl + p`）

管理当前标签页内的面板分割与切换。

| 快捷键 | 说明 |
| :--- | :--- |
| `d` | 向下**水平**分割面板 |
| `r` | 向右**垂直**分割面板 |
| `x` | 关闭当前面板 |
| `f` | **最大化/还原**当前面板（全屏查看） |
| `z` | 开启/关闭**浮动面板** |
| `方向键` | 切换到对应方向的面板 |
| `Tab` | 切换到下一个面板 |
| `e` | 用 `$EDITOR` 编辑当前面板内容 |
| `c` | 重命名当前面板 |
| `Esc` | 返回 Normal 模式 |

---

### Tab 模式（`Ctrl + t`）

管理标签页的新建、切换和重命名。

| 快捷键 | 说明 |
| :--- | :--- |
| `n` | 创建新标签页 |
| `x` | 关闭当前标签页 |
| `r` | 重命名当前标签页 |
| `方向键` / `h` / `l` | 切换到左/右标签页 |
| `1`~`9` | 跳转到对应编号的标签页 |
| `s` | 同步模式（向所有面板广播输入） |
| `Esc` | 返回 Normal 模式 |

---

### Resize 模式（`Ctrl + n`）

调整当前面板的大小。

| 快捷键 | 说明 |
| :--- | :--- |
| `方向键` / `h` / `j` / `k` / `l` | 向对应方向扩大面板 |
| `+` | 增大面板 |
| `-` | 缩小面板 |
| `Esc` | 返回 Normal 模式 |

---

### Scroll 模式（`Ctrl + s`）

滚动查看终端历史输出，支持搜索。

| 快捷键 | 说明 |
| :--- | :--- |
| `方向键` / `j` / `k` | 上下滚动一行 |
| `PageUp` / `PageDown` | 上下翻页 |
| `Ctrl + u` / `Ctrl + d` | 上下滚动半页 |
| `e` | 进入**编辑滚动缓冲区**模式（用 `$EDITOR` 查看） |
| `s` | 开始**搜索**（输入关键词，`n`/`p` 跳转匹配项） |
| `Esc` / `Enter` | 退出滚动模式 |

---

### Session 模式（`Ctrl + o`）

管理和切换会话。

| 快捷键 | 说明 |
| :--- | :--- |
| `d` | **分离**当前会话（Detach，后台保留） |
| `w` | 打开**会话管理器**（可切换会话/标签/面板） |
| `Esc` | 返回 Normal 模式 |

---

## 5. 布局系统

Zellij 支持用 KDL 格式定义布局文件，启动时自动排列面板。

```bash
# 使用内置布局启动
zellij --layout compact      # 紧凑模式，隐藏顶部标签栏
zellij --layout default      # 默认布局

# 使用自定义布局文件
zellij --layout /path/to/my-layout.kdl
```

**自定义布局文件示例** (`~/.config/zellij/layouts/dev.kdl`):

```kdl
layout {
    pane split_direction="vertical" {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
    }
    pane split_direction="horizontal" {
        pane command="nvim" size="60%"
        pane split_direction="vertical" {
            pane command="bash"
            pane command="bash"
        }
    }
    pane size=2 borderless=true {
        plugin location="zellij:status-bar"
    }
}
```

---

## 6. 配置文件

配置文件默认位于 `~/.config/zellij/config.kdl`，可通过以下命令生成：

```bash
zellij setup --dump-config > ~/.config/zellij/config.kdl
```

**常用配置示例**:

```kdl
// 鼠标支持
mouse_mode true

// 复制到系统剪贴板
copy_on_select true

// 主题设置
theme "catppuccin-mocha"

// 默认 Shell
default_shell "zsh"

// 默认布局
default_layout "compact"

// 禁用欢迎提示
simplified_ui true
```

---

## 7. 插件

Zellij 内置了若干实用插件，也支持安装第三方 Wasm 插件。

| 插件 | 说明 |
| :--- | :--- |
| `zellij:status-bar` | 底部状态栏（显示模式与快捷键提示） |
| `zellij:tab-bar` | 顶部标签栏 |
| `zellij:strider` | 侧边文件浏览器 |
| `zellij:compact-bar` | 紧凑版标签栏 |
| `zellij:session-manager` | 会话管理器 |

在 Pane 模式下按 `e` 或通过布局文件可加载插件。

---

## 8. 实用技巧

*   **快速打开浮动终端**：在 Pane 模式下按 `z` 可弹出浮动面板，用完按 `Esc` 隐藏，非常适合临时执行命令。
*   **同步输入到所有面板**：在 Tab 模式下按 `s` 开启同步，可同时向多台服务器执行相同命令。
*   **脚本自动化启动**：将 `zellij attach --create dev` 加入 `.bashrc`/`.zshrc`，每次打开终端自动进入工作会话。

```bash
# 在 .zshrc 中自动连接开发会话
if [[ -z "$ZELLIJ" ]]; then
    zellij attach --create main
fi
```

---

## 9. 与 Tmux 对比速查

| 功能 | Tmux | Zellij |
| :--- | :--- | :--- |
| 水平分割 | `Ctrl+b "` | `Ctrl+p` → `d` |
| 垂直分割 | `Ctrl+b %` | `Ctrl+p` → `r` |
| 新建标签页 | `Ctrl+b c` | `Ctrl+t` → `n` |
| 分离会话 | `Ctrl+b d` | `Ctrl+o` → `d` |
| 进入滚动模式 | `Ctrl+b [` | `Ctrl+s` |
| 配置格式 | `.tmux.conf` | `config.kdl` |
| 插件支持 | 第三方脚本 | 原生 Wasm 插件 |

---

*文档生成日期: 2026-04-24*
