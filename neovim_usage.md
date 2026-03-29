# Neovim 高效使用手册 (Neovim Usage Guide)

Neovim 是 Vim 的高度可扩展重构版本。现代 Neovim 的核心在于使用 **Lua** 进行配置，这比传统的 VimScript 更快且更易维护。

## 1. 基础配置结构
Neovim 的配置文件位于 `~/.config/nvim/init.lua`。建议采用模块化结构：
```text
~/.config/nvim/
├── init.lua          # 入口文件
├── lua/
│   ├── core/         # 基础设置 (options, keymaps)
│   └── plugins/      # 插件配置
└── lazy-lock.json    # 插件锁定文件
```

### 基础选项 (lua/core/options.lua)
```lua
vim.opt.number = true           -- 显示行号
vim.opt.relativenumber = true   -- 相对行号
vim.opt.tabstop = 4             -- Tab 宽度
vim.opt.shiftwidth = 4
vim.opt.expandtab = true        -- 空格替代 Tab
vim.opt.termguicolors = true    -- 真彩色支持 (主题必备)
vim.opt.cursorline = true       -- 高亮当前行
```

## 2. 插件管理 (lazy.nvim)
`lazy.nvim` 是目前 Neovim 社区最主流、速度最快的插件管理器。

### 安装 lazy.nvim
在 `init.lua` 中添加以下代码：
```lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git", "--branch=stable", lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

require("lazy").setup("plugins") -- 自动加载 lua/plugins/ 下的所有文件
```

## 3. 核心插件推荐 (The "Big Three")
### 3.1 语法高亮: nvim-treesitter
提供极速且精准的代码高亮和结构感知。
```lua
-- lua/plugins/treesitter.lua
return {
    "nvim-treesitter/nvim-treesitter",
    build = ":TSUpdate",
    config = function()
        require("nvim-treesitter.configs").setup({
            ensure_installed = { "lua", "python", "javascript", "go", "rust" },
            highlight = { enable = true },
        })
    end
}
```

### 3.2 模糊搜索: telescope.nvim
查找文件、搜索文本、管理 Buffer 的“瑞士军刀”。
- `Ctrl + p` - 查找文件 (需配置快捷键)
- `Live Grep` - 在项目中全局搜索字符串

### 3.3 补全与 LSP: nvim-lspconfig + Mason
- **Mason**: 自动安装 LSP 服务器（如 pyright, gopls）。
- **nvim-cmp**: 代码补全引擎。

## 4. 主题设置 (Themes)
现代 Neovim 推荐使用支持 Treesitter 的主题。

### 热门主题推荐：
1. **Catppuccin** (极简、舒适)
2. **Tokyonight** (深夜霓虹风格)
3. **Gruvbox.nvim** (经典复古，针对 Lua 重写)

### 设置示例 (以 Catppuccin 为例)：
```lua
-- lua/plugins/theme.lua
return {
  "catppuccin/nvim",
  name = "catppuccin",
  priority = 1000, -- 确保主题优先加载
  config = function()
    vim.cmd.colorscheme("catppuccin-mocha") -- 推荐 mocha 或 macchiato
  end,
}
```

## 5. 常用快捷键建议
建议在 `lua/core/keymaps.lua` 中定义：
```lua
local keymap = vim.keymap.set
vim.g.mapleader = " " -- 设置空格为 Leader 键

-- 窗口跳转
keymap("n", "<C-h>", "<C-w>h")
keymap("n", "<C-j>", "<C-w>j")
keymap("n", "<C-k>", "<C-w>k")
keymap("n", "<C-l>", "<C-w>l")

-- 文件树 (需安装 nvim-tree)
keymap("n", "<leader>e", ":NvimTreeToggle<CR>")

-- Telescope
keymap("n", "<leader>ff", ":Telescope find_files<CR>")
keymap("n", "<leader>fg", ":Telescope live_grep<CR>")
```

## 6. 进阶学习资源
- `:help lua-guide` - Neovim 官方 Lua 指南
- **NvChad / AstroNvim / LazyVim** - 三大主流配置框架，如果你不想从零开始，可以直接使用它们。

---
*文档生成日期: 2026-03-30*
