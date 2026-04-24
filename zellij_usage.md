# Zellij 使用指南

`zellij` 是一款用 Rust 编写的现代终端复用器，内置标签页、分屏、浮动窗格与插件系统，开箱即用，无需繁琐配置。

---

## 1. 安装

```bash
# macOS (Homebrew)
brew install zellij

# Arch Linux
pacman -S zellij

# cargo (通用)
cargo install --locked zellij
```

启动：
```bash
zellij
```

---

## 2. 核心概念

| 概念 | 说明 |
| :--- | :--- |
| **Session（会话）** | 最顶层的工作区，可在后台保持运行 |
| **Tab（标签页）** | 会话内的独立工作区，类似浏览器标签 |
| **Pane（窗格）** | 标签页内的分屏单元，每个窗格运行独立终端 |
| **Floating Pane（浮动窗格）** | 悬浮在其他窗格之上的窗格，适合临时查阅 |

---

## 3. 模式系统

Zellij 使用**模式**来分类快捷键，当前模式显示在底部状态栏。

| 模式 | 进入方式 | 作用 |
| :--- | :--- | :--- |
| `Normal` | 默认 / `Esc` | 日常输入 |
| `Pane` | `Ctrl + p` | 管理窗格 |
| `Tab` | `Ctrl + t` | 管理标签页 |
| `Resize` | `Ctrl + n` | 调整窗格大小 |
| `Move` | `Ctrl + h` | 移动窗格位置 |
| `Scroll` | `Ctrl + s` | 滚动 / 复制内容 |
| `Session` | `Ctrl + o` | 管理会话 |
| `Search` | `Ctrl + s` → `e` | 搜索滚动缓冲区 |
| `Locked` | `Ctrl + g` | 锁定所有快捷键（直通模式） |

> **退出任意模式** → 按 `Esc` 或 `Enter` 返回 `Normal`

---

## 4. 窗格操作 (Pane 模式：`Ctrl + p`)

进入 Pane 模式后：

| 按键 | 说明 |
| :--- | :--- |
| `d` | 向右新建垂直分屏 |
| `D` | 向下新建水平分屏 |
| `h / ←` | 聚焦左侧窗格 |
| `l / →` | 聚焦右侧窗格 |
| `j / ↓` | 聚焦下方窗格 |
| `k / ↑` | 聚焦上方窗格 |
| `x` | 关闭当前窗格 |
| `z` | 最大化 / 还原当前窗格 |
| `r` | 重命名当前窗格 |
| `w` | 切换为浮动窗格 |
| `e` | 在编辑器中打开当前窗格内容 |
| `f` | 新建浮动窗格 |
| `c` | 将当前窗格转换为全屏浮动窗格 |

---

## 5. 标签页操作 (Tab 模式：`Ctrl + t`)

进入 Tab 模式后：

| 按键 | 说明 |
| :--- | :--- |
| `n` | 新建标签页 |
| `x` | 关闭当前标签页 |
| `r` | 重命名当前标签页 |
| `h / ←` | 切换到上一个标签页 |
| `l / →` | 切换到下一个标签页 |
| `1` ~ `9` | 直接跳到对应编号的标签页 |
| `s` | 同步输入到当前标签页所有窗格（广播模式） |
| `b` | 将当前窗格断开并新建标签页 |

---

## 6. 调整大小 (Resize 模式：`Ctrl + n`)

进入 Resize 模式后，用方向键或 hjkl 调整当前窗格的大小：

| 按键 | 说明 |
| :--- | :--- |
| `h / ←` | 向左缩小 |
| `l / →` | 向右扩大 |
| `j / ↓` | 向下扩大 |
| `k / ↑` | 向上缩小 |
| `+` | 增大窗格 |
| `-` | 缩小窗格 |

---

## 7. 滚动与搜索 (Scroll 模式：`Ctrl + s`)

| 按键 | 说明 |
| :--- | :--- |
| `↑ / k` | 向上滚动一行 |
| `↓ / j` | 向下滚动一行 |
| `PageUp / u` | 向上翻页 |
| `PageDown / d` | 向下翻页 |
| `e` | 进入搜索模式（输入关键词后 `Enter` 确认） |
| `n` | 下一处搜索结果 |
| `p` | 上一处搜索结果 |
| `s` | 开始复制选区（类 Vi 的块选） |

---

## 8. 会话管理 (Session 模式：`Ctrl + o`)

### 命令行操作

```bash
zellij ls                        # 列出所有会话
zellij attach <name>             # 连接到指定会话
zellij kill-session <name>       # 删除指定会话
zellij kill-all-sessions         # 删除所有会话
zellij --session <name>          # 启动并指定会话名称
zellij -l compact                # 以 compact 布局启动
```

### 会话模式内快捷键

| 按键 | 说明 |
| :--- | :--- |
| `d` | 分离当前会话（Detach，后台保留） |
| `w` | 打开会话管理器（浏览 / 切换所有会话） |
| `q` | 退出 Zellij（结束当前会话） |

---

## 9. 布局 (Layouts)

Zellij 支持用 KDL 格式的布局文件一键还原工作区：

```bash
zellij --layout /path/to/layout.kdl
```

示例布局文件 `dev.kdl`：
```kdl
layout {
    pane split_direction="vertical" {
        pane { command "nvim"; }
        pane split_direction="horizontal" {
            pane { command "bash"; }
            pane { command "btop"; }
        }
    }
    pane size=2 borderless=true {
        plugin location="zellij:status-bar"
    }
}
```

内置布局：
```bash
zellij -l default      # 默认布局（带状态栏）
zellij -l compact      # 紧凑布局（隐藏状态栏）
zellij -l strider      # 文件管理器侧边栏布局
```

---

## 10. 常用配置 (~/.config/zellij/config.kdl)

```kdl
// 修改默认 Shell
default_shell "fish"

// 关闭启动欢迎界面
simplified_ui true

// 鼠标支持（默认已开启）
mouse_mode true

// 将 Ctrl+Space 设为切换 Locked 模式（替代 Ctrl+g）
keybinds {
    normal {
        bind "Ctrl Space" { SwitchToMode "Locked"; }
    }
}

// 主题
theme "catppuccin-mocha"
```

生成默认配置文件：
```bash
zellij setup --dump-config > ~/.config/zellij/config.kdl
```

---

## 11. 与 tmux 快速对比

| 功能 | Zellij | Tmux |
| :--- | :--- | :--- |
| 上手难度 | ⭐ 低（内置 UI 提示） | ⭐⭐⭐ 较高 |
| 配置语言 | KDL | 自定义 DSL |
| 浮动窗格 | ✅ 原生支持 | ❌ 需插件 |
| 插件系统 | ✅ WASM 插件 | ✅ 外部插件 |
| 性能 | 稍高内存占用 | 极轻量 |
| 会话持久化 | ✅ 支持 | ✅ 支持 |

---

*文档生成日期: 2026-04-24*
