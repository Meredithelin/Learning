# bat 使用手册

`bat` 是一个带有语法高亮和 Git 集成的 `cat` 克隆版，旨在提供更美观、更实用的文件查看体验。

## 1. 核心特性

- **语法高亮**：自动识别并高亮显示多种编程语言和标记语言的语法。
- **Git 集成**：在侧边栏显示文件的修改情况（增加、删除、修改）。
- **自动分页**：如果文件内容超过屏幕高度，会自动调用分页器（如 `less`）。
- **显示不可见字符**：可以使用 `-A` 选项查看空格、制表符和换行符。

## 2. 基础用法

### 2.1 查看文件内容
这是最基本的用法，类似于 `cat`：
```bash
bat file.txt
```

### 2.2 一次查看多个文件
```bash
bat file1.md file2.md
```

### 2.3 管道输入
接收来自其他命令的输出并进行高亮处理：
```bash
curl -s https://example.com | bat -l html
```

## 3. 常用选项

| 选项 | 说明 | 示例 |
| :--- | :--- | :--- |
| `-p`, `--plain` | 只显示文本，不显示行号、网格等装饰 | `bat -p file.md` |
| `-n`, `--number` | 只显示行号 | `bat -n file.py` |
| `-l`, `--language` | 手动指定语法高亮语言 | `bat -l json config.txt` |
| `-H`, `--highlight-line` | 高亮指定行或行范围 | `bat -H 10:20 file.rs` |
| `-r`, `--line-range` | 只打印指定范围的行 | `bat -r 5:15 file.go` |
| `-d`, `--diff` | 只显示相对于 Git 索引修改过的行 | `bat -d file.js` |
| `-A`, `--show-all` | 显示不可见字符 | `bat -A file.txt` |
| `--list-languages` | 列出所有受支持的语言 | `bat --list-languages` |
| `--list-themes` | 列出所有可用的主题 | `bat --list-themes` |

## 4. 进阶示例

### 4.1 与 `find` 或 `fd` 配合使用
查看所有搜索到的 Python 文件：
```bash
fd -e py -x bat
```

### 4.2 设置主题
临时使用特定主题：
```bash
bat --theme="Monokai Extended" file.py
```
*注：你可以设置 `BAT_THEME` 环境变量来永久更改主题。*

### 4.3 禁用分页器
如果你希望 `bat` 像 `cat` 一样直接输出所有内容，不启动分页：
```bash
bat --paging=never file.log
```

---
*文档生成日期: 2026-03-21*
