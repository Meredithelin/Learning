# Vim 高效使用手册 (速查版)

这份手册是 `Learning VIM.md` 的精简补充版，旨在提供最常用命令的快速参考及进阶配置建议。

## 1. 核心生存命令 (必须掌握)
- `i` - 进入插入模式
- `Esc` - 返回普通模式
- `:w` - 保存
- `:q!` - 不保存强制退出
- `:wq` 或 `ZZ` - 保存并退出
- `u` - 撤销 (Undo)
- `Ctrl + r` - 重做 (Redo)
- `.` - 重复上一个修改命令

## 2. 快速移动 (光标跳转)
- `H / M / L` - 屏幕 顶部/中间/底部
- `zt / zz / zb` - 将当前行置于屏幕 顶部/中间/底部
- `%` - 在配对的括号 `()`, `[]`, `{}` 之间跳转
- `*` / `#` - 跳转到下一个/上一个相同的单词
- `gd` - 跳转到局部变量定义 (Go to Definition)
- `gf` - 打开光标下的文件名 (Go to File)

## 3. 文本编辑“黑魔法”
- `ci"` - 修改双引号内的内容 (Change Inside ")
- `da(` - 删除括号及其内部内容 (Delete Around ()
- `yiw` - 复制当前单词 (Yank Inside Word)
- `guw` / `gUw` - 将当前单词转为 小写/大写
- `J` - 合并当前行与下一行
- `Ctrl + a` / `Ctrl + x` - 数字 加 1 / 减 1

## 4. 多文件与多窗口
- `:e filename` - 在当前窗口打开文件
- `:vsp` / `:sp` - 垂直/水平分割窗口
- `Ctrl + w + h/j/k/l` - 在窗口间切换
- `Ctrl + w + r` - 交换窗口位置
- `:ls` - 列出当前所有缓冲区 (Buffers)
- `:bN` - 跳转到第 N 个缓冲区 (例如 `:b2`)

## 5. 搜索与替换
- `/pattern` - 向下搜索
- `?pattern` - 向上搜索
- `:%s/old/new/g` - 全局替换
- `:%s/old/new/gc` - 全局确认替换
- `:noh` - 取消当前的搜索高亮

## 6. 配置建议 (.vimrc)
建议在 `~/.vimrc` 中添加以下基础配置：
```vim
syntax on            " 开启语法高亮
set number           " 显示行号
set relativenumber   " 显示相对行号（极力推荐，方便跳转）
set tabstop=4        " 设置 Tab 宽度为 4
set shiftwidth=4     " 设置缩进宽度为 4
set expandtab        " 将 Tab 转换为空格
set cursorline       " 高亮当前行
set ignorecase       " 搜索时忽略大小写
set smartcase        " 如果搜索词包含大写，则不忽略大小写
set incsearch        " 输入搜索词时实时匹配
```

## 7. 插件管理 (推荐使用 vim-plug)
在 `.vimrc` 中配置：
```vim
call plug#begin()
Plug 'preservim/nerdtree'    " 目录树插件
Plug 'vim-airline/vim-airline' " 状态栏增强
Plug 'tpope/vim-surround'    " 快速操作成对符号
Plug 'neoclide/coc.nvim', {'branch': 'release'} " 强大的补全引擎
call plug#end()
```

---
*文档生成日期: 2026-03-21*
*更多详细内容请参考同目录下的 `Learning VIM.md`*
