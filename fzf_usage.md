# fzf (Fuzzy Finder) 使用指南

`fzf` 是一个通用的命令行模糊查找器，能够极大地提升你在终端处理文件、历史记录、进程等的效率。

---

## 1. 核心快捷键 (默认安装后激活)

如果你在安装时允许了快捷键绑定，以下是最常用的组合：

*   **`CTRL-T`**: 在当前目录下查找文件，并将选择的文件路径粘贴到命令行。
*   **`CTRL-R`**: 搜索命令历史记录。支持模糊匹配，按回车即可执行或编辑。
*   **`ALT-C`**: 模糊查找目录，并自动 `cd` 进入该目录。

---

## 2. 基本用法

`fzf` 默认会从标准输入读取列表，并将选择结果输出到标准输出。

*   **简单启动**:
    ```bash
    fzf
    ```
*   **配合其他命令使用 (过滤)**:
    ```bash
    find . -name "*.md" | fzf
    ps -ef | fzf
    ```
*   **作为文件选择器**:
    ```bash
    vim $(fzf)  # 模糊查找文件并用 vim 打开
    ```

---

## 3. 搜索语法

在 `fzf` 的输入框中，你可以使用以下特殊符号：

| 模式 | 匹配类型 | 示例 |
| :--- | :--- | :--- |
| `sbtr` | 模糊匹配 | 匹配包含 s, b, t, r 的项 |
| `'wild` | 精确匹配 | 匹配包含 "wild" 的项 |
| `^music` | 前缀匹配 | 以 "music" 开头的项 |
| `.mp3$` | 后缀匹配 | 以 ".mp3" 结尾项 |
| `!fire` | 反向匹配 | 不包含 "fire" 的项 |
| `^core$ ` | 全匹配 | 精确匹配 "core" |

---

## 4. 高级功能

*   **多选模式**:
    使用 `-m` 参数开启多选。在界面中用 `Tab` 键勾选，`Shift-Tab` 取消勾选。
    ```bash
    fzf -m
    ```
*   **预览窗口**:
    在查找文件时实时预览内容（需要安装 `cat` 或 `bat`）：
    ```bash
    fzf --preview 'cat {}'
    # 或者使用更漂亮的 bat (如果已安装)
    fzf --preview 'bat --style=numbers --color=always --line-range :500 {}'
    ```

---

## 5. 常用配置 (环境变量)

你可以将这些配置添加到你的 `.bashrc` 或 `.zshrc` 中：

```bash
# 修改默认搜索命令（例如排除 .git 目录，需要安装 fd 或 ripgrep）
export FZF_DEFAULT_COMMAND='find . -type f -not -path "*/.*"'

# 设置默认选项（如配色、布局、预览位置）
export FZF_DEFAULT_OPTS="--height 40% --layout=reverse --border --inline-info"
```

---

## 6. 实用示例

*   **杀死进程**:
    ```bash
    kill -9 $(ps -ef | fzf | awk '{print $2}')
    ```
*   **检出 Git 分支**:
    ```bash
    git checkout $(git branch | fzf)
    ```
*   **快速进入项目目录**:
    ```bash
    cd $(find ~/projects -maxdepth 1 -type d | fzf)
    ```
