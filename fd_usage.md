# fd 使用手册

`fd` 是一个简单、快速且用户友好的 `find` 替代品，旨在提供比传统 `find` 更快的搜索速度和更直观的语法。

## 1. 安装指南

在 Ubuntu/Debian 系统上，你可以通过以下命令安装：
```bash
sudo apt update
sudo apt install fd-find
```
*注：在某些系统中，命令名为 `fdfind`，你可以通过 `alias fd='fdfind'` 设置别名。*

## 2. 基础用法

### 2.1 简单搜索
搜索包含 "pattern" 的文件名：
```bash
fd pattern
```

### 2.2 指定目录搜索
在 `path` 目录下搜索：
```bash
fd pattern /path/to/search
```

### 2.3 按扩展名搜索
搜索所有 `.md` 文件：
```bash
fd -e md
```

## 3. 常用选项

| 选项 | 说明 | 示例 |
| :--- | :--- | :--- |
| `-e`, `--extension` | 过滤文件扩展名 | `fd -e jpg` |
| `-H`, `--hidden` | 包含隐藏文件 | `fd -H config` |
| `-I`, `--no-ignore` | 不忽略 `.gitignore` 中的文件 | `fd -I secret` |
| `-L`, `--follow` | 跟随符号链接 | `fd -L mylink` |
| `-p`, `--full-path` | 匹配完整路径而不仅仅是文件名 | `fd -p /home/user` |
| `-t`, `--type` | 指定文件类型 (f: 文件, d: 目录, l: 符号链接) | `fd -t d mydir` |
| `-x`, `--exec` | 对每个结果执行命令 | `fd -e jpg -x convert {} {.}.png` |

## 4. 进阶示例

### 4.1 使用正则表达式
`fd` 默认使用正则表达式：
```bash
fd '^test.*\.py$'
```

### 4.2 执行命令
为搜索到的所有 `.js` 文件执行 `ls -l`：
```bash
fd -e js -x ls -l
```

### 4.3 排除特定目录
搜索时排除 `node_modules` 目录：
```bash
fd pattern -E node_modules
```

### 4.4 搜索最近修改的文件
搜索过去 10 分钟内修改过的文件：
```bash
fd --changed-within 10m
```

---
*文档生成日期: 2026-03-21*
