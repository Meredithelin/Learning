# Linux 命令行核心工具与进阶用法指南

本手册涵盖了常用的文本处理、文件管理及系统工具，并提供了从基础到高级的拓展用法。

---

## 1. 文本处理与流处理 (Text Processing)

### `sort` - 排序
*   **基础**: `sort file.txt` (按字母顺序排序)
*   **常用选项**:
    *   `-n`: 按数值大小排序。
    *   `-r`: 逆序排序。
    *   `-k <col>`: 指定按第几列排序。
    *   `-u`: 去重排序（同 `sort | uniq`）。
    *   `-t <char>`: 指定字段分隔符（默认是空格/制表符）。
*   **拓展**: `sort -t: -k3,3n /etc/passwd` (按用户 ID 的数值大小对 passwd 文件排序)。

### `uniq` - 去重 (需先排序)
*   **基础**: `uniq file.txt` (去除相邻的重复行)
*   **常用选项**:
    *   `-c`: 在行首显示该行出现的次数。
    *   `-d`: 仅显示重复的行。
    *   `-u`: 仅显示不重复的行。
*   **进阶**: `sort file.txt | uniq -c | sort -nr` (统计词频并按降序排列)。

### `head` & `tail` - 查看头部/尾部内容
*   **基础**: `head -n10 file` / `tail -n10 file`
*   **常用选项**:
    *   `tail -f`: 实时滚动查看文件新增内容（常用于看日志）。
    *   `tail -n +K`: 从第 K 行开始显示。
*   **拓展**: `tail -n +2 file.csv` (跳过 CSV 文件的表头)。

### `wc` - 统计
*   **基础**: `wc -l file` (统计行数)
*   **常用选项**:
    *   `-w`: 统计单词数。
    *   `-c`: 统计字节数。
    *   `-m`: 统计字符数。

### `cut` - 剪切列 (新增)
*   **用法**: `cut -d',' -f1,3 file.csv` (以逗号分隔，提取第1和第3列)。

### `tr` - 替换/删除字符 (新增)
*   **用法**: `echo "hello" | tr 'a-z' 'A-Z'` (转大写)。
*   **常用**: `tr -d '\r' < dos_file.txt > unix_file.txt` (删除回车符)。

---

## 2. 正则搜索与流编辑 (Regex & Stream Editing)

### `grep` - 文本搜索
*   **基础**: `grep "pattern" file`
*   **常用选项**:
    *   `-i`: 忽略大小写。
    *   `-v`: 反向选择（显示不匹配的行）。
    *   `-n`: 显示行号。
    *   `-r`: 递归搜索目录下所有文件。
    *   `-E`: 支持扩展正则表达式 (ERE)。
    *   `-o`: 只输出匹配到的部分，而不是整行。
*   **进阶**: `grep -r "TODO" . --exclude-dir=.git` (在当前目录递归搜索 TODO，排除 .git 目录)。

### `sed` - 流编辑器
*   **基础**: `sed 's/old/new/g' file` (全局替换并在屏幕打印)
*   **常用选项**:
    *   `-i`: **直接修改文件内容** (慎用)。
    *   `-n`: 配合 `p` 命令只打印匹配行。
*   **拓展**:
    *   `sed -i.bak 's/foo/bar/g' file`: 替换并生成备份文件。
    *   `sed '3,5d' file`: 删除第 3 到第 5 行。

### `awk` - 强大的文本报告工具
*   **基础**: `awk '{print $1}' file` (打印第一列)
*   **进阶**:
    *   `awk -F: '$3 > 1000 {print $1}' /etc/passwd`: 以 `:` 为分隔符，打印 UID 大于 1000 的用户名。
    *   `awk '{sum += $1} END {print sum}' data.txt`: 计算第一列的总和。

---

## 3. 文件查找与管理 (File Management)

### `find` - 查找文件
*   **基础**: `find . -name "*.txt"`
*   **进阶**:
    *   `find /var/log -type f -mtime +7`: 查找 7 天前修改过的普通文件。
    *   `find . -type f -size +100M`: 查找大于 100M 的文件。
    *   `find . -type f -name "*.log" -exec rm {} \;`: 查找并删除所有 .log 文件。
    *   `find . -type f -name "*.py" | xargs grep "import"`: 配合 `xargs` 高效处理大量文件。

---

## 4. 计算、绘图与数学 (Math & Tools)

### `bc` - 任意精度计算器
*   **用法**: `echo "scale=2; 10 / 3" | bc -l` (保留两位小数)。

### `paste` - 合并文件行
*   **用法**: `paste file1 file2` (并行合并)
*   **拓展**: `paste -sd+ file | bc` (将文件中的所有行用 `+` 连接并求和)。

### `R` & `gunplot` - 数据分析与绘图
*   **R**: 在终端输入 `R` 进入交互式环境。
    *   一行流: `Rscript -e "print(mean(c(1,2,3)))"`
*   **gunplot**: 强大的绘图工具。
    *   `plot "data.txt" with lines`

---

## 5. 其他实用技巧

*   **`$_`**: 引用上一个命令的最后一个参数。
    *   `mkdir test_dir && cd $_`
*   **`sleep`**: 延迟执行。
    *   `sleep 5 && echo "Done"`
*   **`xargs`**: 将标准输入转为命令行参数。
    *   `cat urls.txt | xargs -n1 curl -O` (下载列表中的所有 URL)

---
*Last updated: 2026-03-22*
