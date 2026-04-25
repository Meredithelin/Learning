# GitHub Copilot 使用教程

GitHub Copilot 是一款由 AI 驱动的编程助手，由 GitHub 与 OpenAI 联合开发。它不仅能在编辑器中实时提供代码建议，还深度集成在 GitHub.com 网站上，覆盖从代码编写、代码审查到 Issue 讨论的完整开发流程。

---

## 第一部分：在编辑器中使用 Copilot（以 VS Code 为例）

### 1. 安装与激活

1. 打开 VS Code，进入扩展商店（`Ctrl + Shift + X`）。
2. 搜索 **GitHub Copilot** 并点击安装。同时建议安装 **GitHub Copilot Chat** 扩展。
3. 安装完成后，点击右下角的 Copilot 图标，选择 **Sign in to GitHub** 完成授权登录。
4. 登录成功后，状态栏出现 Copilot 图标即表示已激活。

> **前提**：你需要有一个有效的 GitHub Copilot 订阅（个人版、商业版或教育版）。

---

### 2. 核心功能

#### 2.1 代码自动补全 (Inline Suggestions)
当你开始编写代码时，Copilot 会根据上下文自动给出灰色半透明的建议。
- **接受建议**：按 `Tab` 键。
- **接受部分建议**：按 `Ctrl` + `→` (Windows/Linux) 或 `Cmd` + `→` (macOS) 逐词接受。
- **查看下一个/上一个建议**：按 `Alt` + `]` 或 `Alt` + `[`。
- **拒绝建议**：继续输入或按 `Esc`。

#### 2.2 聊天对话 (GitHub Copilot Chat)
在侧边栏点击 Copilot 图标，或者在编辑器中按 `Ctrl` + `I` (Windows/Linux) 或 `Cmd` + `I` (macOS) 唤起内联聊天。
- **代码解释**：选中一段代码，输入 `/explain`，它会逐行解析逻辑。
- **修复 Bug**：输入 `/fix`，它会分析错误并给出修复方案。
- **编写测试**：输入 `/tests`，它会为当前函数生成单元测试。
- **优化代码**：输入 `/optimize`，它会提供性能更好的写法。

#### 2.3 自然语言转代码 (Comments to Code)
写一段注释，描述你想实现的功能，然后按回车。
```javascript
// 写一个函数，计算两个日期之间的天数
function 
```
Copilot 会自动补全函数体。

---

### 3. 常用斜杠命令 (Slash Commands)
在 Copilot Chat 中使用斜杠命令可以更精确地执行特定任务：
- `/help`：获取关于 GitHub Copilot 的帮助。
- `/doc`：为选中的代码添加文档注释（如 JSDoc）。
- `/clear`：清空当前的聊天会话。
- `/workspace`：在整个工作区搜索并回答问题（需要打开文件夹）。
- `/new`：根据自然语言描述，搭建一个新项目的脚手架结构。

---

### 4. 编辑器快捷键速查表

| 功能 | Windows/Linux | macOS |
| :--- | :--- | :--- |
| 接受建议 | `Tab` | `Tab` |
| 逐词接受 | `Ctrl + →` | `Cmd + →` |
| 拒绝建议 | `Esc` | `Esc` |
| 切换下一个建议 | `Alt + ]` | `Option + ]` |
| 切换上一个建议 | `Alt + [` | `Option + [` |
| 唤起内联聊天 | `Ctrl + I` | `Cmd + I` |
| 打开 Copilot 侧边聊天 | `Ctrl + Shift + I` | `Cmd + Shift + I` |
| 打开 Copilot 建议面板 | `Ctrl + Enter` | `Ctrl + Enter` |

---

## 第二部分：在 GitHub.com 网站上使用 Copilot

Copilot 不只在编辑器里工作——它也深度集成在 GitHub 网站上，让你在浏览器中就能享受 AI 辅助。

### 5. GitHub.com 上的 Copilot Chat（侧边栏）

在 GitHub.com 任意页面，点击右上角的 **Copilot 图标**（黑色星形图标），可以打开全局 Copilot Chat 侧边栏。

**你可以问它：**
- **解释代码**：把代码粘贴进去，直接问"这段代码是做什么的？"
- **仓库问题**：在仓库页面打开 Chat，问"这个项目是如何处理身份验证的？"
- **通用编程问题**：不依赖任何仓库，像普通 AI 聊天一样提问。

**技巧**：在 GitHub Chat 中使用 `@` 来引用上下文：
- `@github`：让 Copilot 搜索整个 GitHub。
- `#file:README.md`：引用当前仓库中的某个文件作为上下文。

---

### 6. Pull Request（PR）中的 Copilot

这是 GitHub.com 上 Copilot 最强大的功能之一，极大提升代码审查效率。

#### 6.1 自动生成 PR 描述
创建 PR 时，在标题和描述输入框的右上角，点击 **"Copilot actions" → "Summary"** 按钮。Copilot 会分析这次 PR 涉及的所有文件变更，自动撰写一份结构化的 PR 摘要，包括：
- 改动了什么
- 为什么要改（根据 commit message 推断）
- 受影响的文件列表

#### 6.2 在 PR 中进行对话（Copilot Chat in PR）
在已打开的 PR 页面，点击右上角的 Copilot 图标，可以直接针对这个 PR 的代码变更提问：
- "这次变更对性能有什么影响？"
- "这里为什么要用 async/await？"
- "帮我找出这次 PR 中潜在的 Bug。"

#### 6.3 Copilot Code Review（代码审查）
在 PR 页面，点击右侧 **"Reviewers"** 区域旁边的 **"Request Copilot review"**，Copilot 会扮演一个代码审查者，自动在代码行上留下审查评论，指出潜在问题、建议改进。

---

### 7. Issues 中的 Copilot

在 Issue 详情页，点击 Copilot 图标，可以：
- **总结 Issue 讨论**：对于有大量评论的长 Issue，让 Copilot 生成一个摘要，快速了解当前进展和争议点。
- **建议解决方案**：描述 Bug 或需求，Copilot 可以给出解决思路甚至代码片段。

---

### 8. 在 GitHub.com 上直接编辑代码（github.dev）

在任意 GitHub 仓库页面，按下 `.`（英文句号键），会在浏览器中打开一个完整的 VS Code 编辑器（`github.dev`），同样支持 Copilot 的所有编辑器功能（自动补全、Chat等），无需在本地安装任何软件。

---

## 第三部分：最佳实践

### 9. 写出更好的 Prompt

#### 9.1 给它清晰的上下文
Copilot 就像"读心术"，但它需要上下文。
- **保持相关文件打开**：Copilot 会参考你当前打开的其他标签页中的代码。
- **编写有意义的函数名和变量名**：清晰的命名有助于它理解你的意图。

#### 9.2 编写高质量的注释
越具体越好：
- *❌ 坏例子*：`// 排序数组`
- *✅ 好例子*：`// 使用快速排序算法对包含对象的数组按 'id' 属性进行升序排列`

#### 9.3 用自然语言描述需求，逐步迭代
不要指望一次生成完美代码。先让 Copilot 给出一个基础实现，然后继续追问："现在给这个函数添加错误处理"，"把它改成支持异步操作"。

### 10. 验证 AI 的输出

**记住：AI 也会写出错误的或有安全漏洞的代码。**
- 始终运行现有的测试套件，确保 Copilot 的建议没有破坏原有功能。
- 对于涉及安全、权限、数据库操作的代码，必须人工仔细审查。
- 像对待初级实习生的代码一样进行 Code Review——信任但要验证。

---

*更新于: 2026-04-01*
