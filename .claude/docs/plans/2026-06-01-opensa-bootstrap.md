# openSA v0.1 骨架实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在 `D:\workspace\github\openSA` 初始化一个 openSA 项目骨架，搭建完整的 mdBook 文档站点 + GitHub Actions 自动部署到 GitHub Pages，可作为 v0.1 MVP 验收。

**Architecture:**
- 静态站点用 Rust 生态的 mdBook 生成，源在 `book/src/`
- 仓库主分支 `main` push 后由 GitHub Actions 自动构建并部署到 `gh-pages` 分支，再由 GitHub Pages 服务
- 内容用中文，分两大部分：基础/进阶能力章节（占位）+ 案例库（独立 section）

**Tech Stack:**
- Rust 工具链（rustup / cargo）
- mdBook（`cargo install mdbook`）
- GitHub Actions（`actions/checkout@v4` + `taiki-e/install-action@v2` + `actions/deploy-pages@v4`）
- Git / GitHub

**Spec:** 详见 `.claude/docs/specs/2026-06-01-opensa-bootstrap-design.md`

---

## 实施前置

> 工程师在开始 Task 1 之前必须知道的事实：
- 工作目录：`D:\workspace\github\openSA`
- 操作系统：Windows 11（PowerShell 7+）
- 平台：rust 已就绪（路径中 `C:\Users\Administrator\.cargo\bin` 存在）
- 仓库 owner：`<owner>` 占位符在 Task 5 阶段回填
- 计划文件：本文档
- 设计文档：`.claude/docs/specs/2026-06-01-opensa-bootstrap-design.md`

---

## Task 1: 验证与准备工具链

**Files:** 不创建文件，纯环境检查。

- [ ] **Step 1: 验证当前目录**

```powershell
Set-Location D:\workspace\github\openSA
Get-Location
Get-ChildItem -Force
```

预期：`Path = D:\workspace\github\openSA`，目录内容为空（除 `.` 和 `..`）。

- [ ] **Step 2: 验证 git 与 cargo 可用**

```powershell
git --version
cargo --version
```

预期：两行版本输出（无错误）。失败时中止并提示用户安装。

- [ ] **Step 3: 验证 mdbook 是否已安装**

```powershell
mdbook --version
```

预期：版本号字符串。**若报错**，进入 Step 4；**若成功**跳到 Task 2。

- [ ] **Step 4: 安装 mdbook（仅在 Step 3 失败时执行）**

```powershell
cargo install mdbook
```

预期：cargo 编译并安装到 `~/.cargo/bin/mdbook.exe`。这一步**可能耗时 5-10 分钟**。

- [ ] **Step 5: 再次验证 mdbook**

```powershell
mdbook --version
```

预期：`mdbook v0.4.x`（或更高）版本号。

---

## Task 2: 初始化 git 仓库与 .gitignore

**Files:**
- Create: `D:\workspace\github\openSA\.gitignore`

- [ ] **Step 1: git init**

```powershell
Set-Location D:\workspace\github\openSA
git init -b main
```

预期：`Initialized empty Git repository in D:/workspace/github/openSA/.git/`。

- [ ] **Step 2: 配置 git 用户（如果尚未配置）**

```powershell
git config user.name
git config user.email
```

预期：输出非空。**若为空**，执行：

```powershell
git config user.name "openSA"
git config user.email "opensa@example.com"
```

（用户可在后续 Task 7 推送前自行修改为真实值。）

- [ ] **Step 3: 写入 .gitignore**

```powershell
'# mdBook 构建产物
/book/book/

# IDE
.idea/
.vscode/
*.swp
*~

# OS
.DS_Store
Thumbs.db

# Rust
/target/
**/*.rs.bk
Cargo.lock.bak

# 临时文件
*.tmp
*.bak' | Set-Content -Path D:\workspace\github\openSA\.gitignore -Encoding UTF8
```

- [ ] **Step 4: 验证 .gitignore 内容**

```powershell
Get-Content D:\workspace\github\openSA\.gitignore
```

预期：看到完整 20 行左右的忽略规则。

- [ ] **Step 5: 提交**

```powershell
git add .gitignore
git commit -m "chore: init git repo with gitignore"
```

预期：`[main (root-commit) ...] chore: init git repo with gitignore`。

---

## Task 3: 写入 LICENSE（MIT）

**Files:**
- Create: `D:\workspace\github\openSA\LICENSE`

- [ ] **Step 1: 写入 MIT 许可证**

写入以下内容到 `D:\workspace\github\openSA\LICENSE`（年份 2026，版权人 "openSA Contributors"）：

```text
MIT License

Copyright (c) 2026 openSA Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

> **实施方式**：使用 Write 工具一次性写入。**不要**用 Set-Content 拼接（避免引号转义问题）。

- [ ] **Step 2: 验证文件**

```powershell
Get-Content D:\workspace\github\openSA\LICENSE | Select-Object -First 5
```

预期：前 5 行匹配上述内容。

- [ ] **Step 3: 提交**

```powershell
git add LICENSE
git commit -m "chore: add MIT license"
```

---

## Task 4: 写入 README.md（GitHub 仓库门面）

**Files:**
- Create: `D:\workspace\github\openSA\README.md`

- [ ] **Step 1: 写入 README.md**

使用 Write 工具，路径 `D:\workspace\github\openSA\README.md`，内容：

````markdown
# openSA · 开源系统架构师

> 一个面向初中级工程师到架构师的中文架构师知识库与真实案例库。

## ✨ 特性

- 🎯 **五大能力维度**：设计 / 决策 / 沟通 / 工程 / 领导
- 🏛️ **真实案例库**：按行业、规模、主题多维索引
- 📈 **渐进式学习路径**：每章标注「入门 / 进阶 / 架构师」分级
- 🤝 **开源协作**：欢迎贡献

## 🌐 在线阅读

https://<owner>.github.io/openSA/

## 🚀 本地构建

前置：Rust 工具链（[rustup](https://rustup.rs/)）

```bash
cargo install mdbook
git clone https://github.com/<owner>/openSA.git
cd openSA/book
mdbook serve --open
```

## 📖 内容导览

| 模块 | 说明 |
|---|---|
| 基础能力 | 设计 / 决策 / 沟通 / 工程 |
| 进阶能力 | 领导 / 跨域整合 / 复杂系统驾驭 |
| 案例库 | 真实世界架构案例 |
| 附录 | 术语表、阅读路径、贡献指南 |

## 🤝 贡献

参见 [CONTRIBUTING.md](./CONTRIBUTING.md)

- **Issue**：选题、纠错、提问
- **PR**：内容补充、案例库贡献

## 📜 许可证

[MIT](./LICENSE)

## 🗺️ 路线图

- **v0.1**：骨架 + 部署通路（当前）
- **v0.2**：基础能力章节首批内容
- **v0.3**：案例库首批 5 个真实案例
- **v0.4**：进阶能力章节
- **v1.0**：内容覆盖完整 / 中级成熟度
````

> **注意**：占位符 `<owner>` 由用户在推送到 GitHub 之前手动替换为自己的用户名（或组织名）。

- [ ] **Step 2: 验证文件**

```powershell
Get-Content D:\workspace\github\openSA\README.md | Select-Object -First 10
```

预期：看到 `# openSA · 开源系统架构师` 标题及导言。

- [ ] **Step 3: 提交**

```powershell
git add README.md
git commit -m "docs: add project README"
```

---

## Task 5: 写入 CONTRIBUTING.md

**Files:**
- Create: `D:\workspace\github\openSA\CONTRIBUTING.md`

- [ ] **Step 1: 写入贡献指南**

使用 Write 工具，路径 `D:\workspace\github\openSA\CONTRIBUTING.md`，内容：

````markdown
# 贡献指南

感谢你关注 openSA！这里是一个开放的架构师知识库，欢迎任何形式的贡献。

## 提交 Issue

- 选题建议：你希望看到的内容主题
- 错误反馈：请附 URL、复现步骤、上下文
- 内容讨论：在现有章节下展开辩论

## 提交 PR

1. Fork → 新分支 → 修改
2. 本地 `cd book && mdbook serve` 自检（确保无 404 / 链接断）
3. PR 描述：动机 / 改动点 / 关联 Issue
4. 案例库贡献必须使用 [TEMPLATE.md](./book/src/20-cases/TEMPLATE.md)

## 内容规范

- 中文为主，技术术语可保留英文
- 每章首部加"读者分级"标签：`[入门]` / `[进阶]` / `[架构师]`
- 引用外部资料附链接
- 配图放在 `assets/`，引用用相对路径

## 行为准则

欢迎、尊重、专注内容本身。技术争论请基于事实与场景。
````

- [ ] **Step 2: 验证文件**

```powershell
Get-Content D:\workspace\github\openSA\CONTRIBUTING.md | Select-Object -First 10
```

预期：看到 `# 贡献指南` 标题。

- [ ] **Step 3: 提交**

```powershell
git add CONTRIBUTING.md
git commit -m "docs: add contributing guide"
```

---

## Task 6: 创建 GitHub Actions workflow

**Files:**
- Create: `D:\workspace\github\openSA\.github\workflows\mdbook.yml`

- [ ] **Step 1: 创建 .github/workflows 目录**

```powershell
New-Item -ItemType Directory -Path D:\workspace\github\openSA\.github\workflows -Force
```

预期：无报错，目录创建成功。

- [ ] **Step 2: 写入 mdbook.yml**

使用 Write 工具，路径 `D:\workspace\github\openSA\.github\workflows\mdbook.yml`，内容：

```yaml
name: Deploy mdBook site to Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: taiki-e/install-action@v2
        with:
          tool: mdbook
      - run: mdbook build
        working-directory: book
      - uses: actions/upload-pages-artifact@v3
        with:
          path: book/book

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 3: 验证 YAML 语法**

```powershell
Get-Content D:\workspace\github\openSA\.github\workflows\mdbook.yml
```

预期：能完整看到 35 行左右的内容。

- [ ] **Step 4: 提交**

```powershell
git add .github/workflows/mdbook.yml
git commit -m "ci: add mdbook deploy workflow"
```

---

## Task 7: 初始化 mdBook 项目结构

**Files:**
- Create: `D:\workspace\github\openSA\book\book.toml`
- Create: `D:\workspace\github\openSA\book\src\`（目录）

- [ ] **Step 1: 创建 book/src 目录**

```powershell
New-Item -ItemType Directory -Path D:\workspace\github\openSA\book\src -Force
```

预期：无报错。

- [ ] **Step 2: 写入 book.toml**

使用 Write 工具，路径 `D:\workspace\github\openSA\book\book.toml`，内容：

```toml
[book]
title = "openSA · 开源系统架构师"
authors = ["openSA Contributors"]
language = "zh"
src = "src"
description = "面向初中级到架构师的中文架构师知识库与真实案例库"

[build]
build-dir = "book"
create-missing = true

[output.html]
default-theme = "light"
preferred-dark-theme = "navy"
git-repository-url = "https://github.com/<owner>/openSA"
edit-url-template = "https://github.com/<owner>/openSA/edit/main/book/{path}"
site-url = "/openSA/"
```

> **注意**：`<owner>` 在 README 中已经使用过同样的占位符，保持一致。

- [ ] **Step 3: 验证 book.toml 解析（mdbook 应当不报错）**

```powershell
Set-Location D:\workspace\github\openSA\book
mdbook test --help  # 验证 mdbook 二进制可用
mdbook build
```

预期：mdbook build 在没有源文件的情况下会创建空的 `book/` 目录，不应报错（`create-missing = true` 已开启）。

- [ ] **Step 4: 检查 build 产物**

```powershell
Get-ChildItem D:\workspace\github\openSA\book\book
```

预期：看到 `index.html` 与 `book` 子目录（即 SUMMARY 缺省首页）。

- [ ] **Step 5: 清理临时 build 产物（占位文件未就绪，build 无意义）**

```powershell
Remove-Item -Recurse -Force D:\workspace\github\openSA\book\book
```

预期：build 目录被删除。

- [ ] **Step 6: 提交**

```powershell
Set-Location D:\workspace\github\openSA
git add book/book.toml
git commit -m "feat(book): init mdbook project with book.toml"
```

---

## Task 8: 写入 SUMMARY.md（章节总览）

**Files:**
- Create: `D:\workspace\github\openSA\book\src\SUMMARY.md`

- [ ] **Step 1: 写入 SUMMARY.md**

使用 Write 工具，路径 `D:\workspace\github\openSA\book\src\SUMMARY.md`，内容：

````markdown
# Summary

[简介](introduction.md)

# 基础能力

- [设计能力](01-design/README.md)
- [决策能力](02-decision/README.md)
- [沟通能力](03-communication/README.md)
- [工程能力](04-engineering/README.md)

# 进阶能力

- [领导能力](05-leadership/README.md)
- [跨域整合](06-integration/README.md)
- [复杂系统驾驭](07-complexity/README.md)

# 案例库

- [案例库导览](20-cases/README.md)
  - [按行业](20-cases/by-industry/README.md)
  - [按规模](20-cases/by-scale/README.md)
  - [按主题](20-cases/by-topic/README.md)
  - [贡献模板](20-cases/TEMPLATE.md)

# 附录

- [术语表](99-appendix/glossary.md)
- [阅读路径](99-appendix/reading-paths.md)
- [贡献指南](99-appendix/contributing.md)
````

- [ ] **Step 2: 验证 mdbook build 会提示缺失文件**

```powershell
Set-Location D:\workspace\github\openSA\book
mdbook build
```

预期：**报错**说"missing chapter"，列出所有 SUMMARY.md 引用的不存在文件。这是预期行为（占位文件尚未创建）。

---

## Task 9: 写入占位 .md 文件

**Files:** 一次性创建以下 16 个文件（每个 1-5 行引导文字）

```
book/src/introduction.md
book/src/01-design/README.md
book/src/02-decision/README.md
book/src/03-communication/README.md
book/src/04-engineering/README.md
book/src/05-leadership/README.md
book/src/06-integration/README.md
book/src/07-complexity/README.md
book/src/20-cases/README.md
book/src/20-cases/by-industry/README.md
book/src/20-cases/by-scale/README.md
book/src/20-cases/by-topic/README.md
book/src/20-cases/TEMPLATE.md
book/src/99-appendix/glossary.md
book/src/99-appendix/reading-paths.md
book/src/99-appendix/contributing.md
```

- [ ] **Step 1: 创建各章节目录**

```powershell
$dirs = @(
    "D:\workspace\github\openSA\book\src\01-design",
    "D:\workspace\github\openSA\book\src\02-decision",
    "D:\workspace\github\openSA\book\src\03-communication",
    "D:\workspace\github\openSA\book\src\04-engineering",
    "D:\workspace\github\openSA\book\src\05-leadership",
    "D:\workspace\github\openSA\book\src\06-integration",
    "D:\workspace\github\openSA\book\src\07-complexity",
    "D:\workspace\github\openSA\book\src\20-cases\by-industry",
    "D:\workspace\github\openSA\book\src\20-cases\by-scale",
    "D:\workspace\github\openSA\book\src\20-cases\by-topic",
    "D:\workspace\github\openSA\book\src\99-appendix"
)
foreach ($d in $dirs) { New-Item -ItemType Directory -Path $d -Force | Out-Null }
Get-ChildItem -Recurse -Directory D:\workspace\github\openSA\book\src
```

预期：看到上述 11 个目录。

- [ ] **Step 2: 写入 introduction.md**

使用 Write 工具，路径 `D:\workspace\github\openSA\book\src\introduction.md`，内容：

````markdown
# 简介

欢迎来到 **openSA**（Open System Architect）—— 一个面向初中级工程师到架构师的中文架构师知识库与真实案例库。

## 为什么写 openSA

中文社区中，系统架构师相关的系统化、可免费阅读的中文资料仍然稀缺。我们希望通过开源协作的方式，沉淀一套：

- **方法论**：五大能力维度 —— 设计、决策、沟通、工程、领导
- **真实案例**：按行业、规模、主题多维索引的实战经验
- **渐进路径**：从入门到架构师，每章标注读者分级

## 如何阅读

- **入门读者**：建议按"基础能力"顺序阅读，做笔记、做思考
- **进阶读者**：挑感兴趣的章节，跳读"进阶能力"与"案例库"
- **架构师**：欢迎贡献内容，特别是案例库

## 如何贡献

参见 [贡献指南](https://github.com/<owner>/openSA/blob/main/CONTRIBUTING.md)。

> 本简介将随项目成长持续迭代。
````

- [ ] **Step 3: 写入 4 个基础能力 README 占位**

为以下文件写入相同模板（替换章节名）：

- `book/src/01-design/README.md`：标题 = `设计能力`
- `book/src/02-decision/README.md`：标题 = `决策能力`
- `book/src/03-communication/README.md`：标题 = `沟通能力`
- `book/src/04-engineering/README.md`：标题 = `工程能力`

模板（以设计能力为例）：

````markdown
# 设计能力

> 本章节待编写。openSA 欢迎贡献者补充。
>
> **建议覆盖**：
> - 系统思维：边界、涌现、反馈循环
> - 抽象与建模：从需求到领域模型
> - 架构模式：典型场景下的可复用方案
>
> 读者分级：[入门] / [进阶] / [架构师]
>
> 参见 [贡献指南](../99-appendix/contributing.md)。
````

- [ ] **Step 4: 写入 3 个进阶能力 README 占位**

为以下文件写入相同模板（替换章节名）：

- `book/src/05-leadership/README.md`：标题 = `领导能力`，建议覆盖 = `技术愿景 / 团队建设 / 影响力`
- `book/src/06-integration/README.md`：标题 = `跨域整合`，建议覆盖 = `性能 / 可用性 / 安全 / 成本`
- `book/src/07-complexity/README.md`：标题 = `复杂系统驾驭`，建议覆盖 = `遗留系统 / 大规模分布式 / 故障应急`

- [ ] **Step 5: 写入案例库相关文件**

- `book/src/20-cases/README.md`：标题 = `案例库导览`，内容：

````markdown
# 案例库导览

> 真实世界架构案例。按三种维度索引：**行业** / **规模** / **主题**。

## 如何贡献

参见 [TEMPLATE.md](./TEMPLATE.md)。每个案例应聚焦于：**背景 → 架构概览 → 关键决策 → 结果与反思**。
````

- `book/src/20-cases/by-industry/README.md`：

````markdown
# 按行业

> 案例按行业分类：电商 / 金融 / 社交 / IoT / 政企 / 媒体 / 教育 / 医疗 / 物流 / 其他。

案例待补充。参见 [TEMPLATE.md](../TEMPLATE.md)。
````

- `book/src/20-cases/by-scale/README.md`：

````markdown
# 按规模

> 案例按业务规模分类：初创（0-10万用户） / 成长（10万-1000万） / 大型（1000万+） / 超大规模（亿级）。

案例待补充。参见 [TEMPLATE.md](../TEMPLATE.md)。
````

- `book/src/20-cases/by-topic/README.md`：

````markdown
# 按主题

> 案例按架构主题分类：微服务 / 中台 / 数据平台 / 高并发 / 容灾 / 多活 / 移动端架构 / AI 工程化。

案例待补充。参见 [TEMPLATE.md](../TEMPLATE.md)。
````

- `book/src/20-cases/TEMPLATE.md`：

````markdown
# 案例标题

## 元信息

- **行业**：
- **规模**：
- **主题**：
- **作者 / 贡献者**：
- **日期**：

## 背景

要解决什么问题？约束条件？

## 架构概览

（含图、文描述）

## 关键决策

| 决策 | 取舍 | 备选方案 |
|---|---|---|
|  |  |  |

## 结果与反思

效果如何？回头看有何得失？

## 参考资料

（可选）
````

- [ ] **Step 6: 写入 3 个附录占位**

- `book/src/99-appendix/glossary.md`：标题 = `术语表`，内容 = `> 待编写。`

- `book/src/99-appendix/reading-paths.md`：标题 = `阅读路径`，内容 = `> 待编写。建议给出三类读者路径：入门 / 进阶 / 架构师。`

- `book/src/99-appendix/contributing.md`：标题 = `贡献指南（站点内）`，内容：

````markdown
# 贡献指南

完整贡献流程参见仓库根目录的 [CONTRIBUTING.md](https://github.com/<owner>/openSA/blob/main/CONTRIBUTING.md)。

## 站点内导航

- [回到 SUMMARY](../SUMMARY.md)
- [术语表](./glossary.md)
- [阅读路径](./reading-paths.md)
````

- [ ] **Step 7: 验证 mdbook build 成功**

```powershell
Set-Location D:\workspace\github\openSA\book
mdbook build
```

预期：**无错误**。如果有"missing chapter"错误，回到对应文件补齐。

- [ ] **Step 8: 验证 build 产物**

```powershell
Get-ChildItem D:\workspace\github\openSA\book\book
Get-ChildItem D:\workspace\github\openSA\book\book -Recurse -Depth 2 | Select-Object FullName
```

预期：看到 `index.html` 与 `案例库/`、`基础能力/`、`进阶能力/`、`附录/` 等目录。

- [ ] **Step 9: 本地服务验证（可选）**

```powershell
mdbook serve --port 3000
```

预期：终端输出 `[INFO] Serving on: http://localhost:3000`。在另一终端用浏览器或 curl 访问该 URL，能看到 mdBook 首页。**完成后用 Ctrl+C 关闭**。

- [ ] **Step 10: 提交所有占位文件**

```powershell
Set-Location D:\workspace\github\openSA
git add book/src
git commit -m "feat(book): add SUMMARY and placeholder chapters"
```

---

## Task 10: 提交 .claude/docs/（可选跟踪）

> 设计文档本身是 AI 工作产物，默认不进 git。本计划 Task 7 起的"提交"已经处理主仓库内容。**本任务可选**，仅当用户希望把 AI 文档也纳入版本控制时执行。

- [ ] **Step 1: 询问用户**

在执行前先问用户："是否把 `.claude/docs/` 也纳入 git 跟踪？"。**默认否**。

- [ ] **Step 2: 若用户同意，添加并提交**

```powershell
git add .claude/docs
git commit -m "docs: include design spec in repo"
```

> 注意：后续 AI 工作笔记应**默认不**纳入。

---

## Task 11: 配置 GitHub remote 并推送

> 推送前的准备动作，由用户（owner）完成。本任务**只给出步骤**，不直接执行推送。

- [ ] **Step 1: 提示用户在 GitHub 创建空仓库**

提示用户：

1. 访问 https://github.com/new
2. 仓库名：`openSA`
3. 描述：`开源系统架构师 · Open System Architect`
4. 选 **Public**
5. **不要**勾选"Add a README file" / "Add .gitignore" / "Choose a license"（我们已经有了）
6. 点击 "Create repository"

- [ ] **Step 2: 添加 remote 并推送**

把 `<owner>` 替换为用户的真实 GitHub 用户名后执行：

```powershell
git remote add origin https://github.com/<owner>/openSA.git
git branch -M main
git push -u origin main
```

预期：所有 commit 推送到 GitHub，main 分支可见。

- [ ] **Step 3: 替换占位符 `<owner>`**

```powershell
$old = "<owner>"
$new = "<用户的 GitHub 用户名>"
$files = @(
    "D:\workspace\github\openSA\README.md",
    "D:\workspace\github\openSA\book\book.toml",
    "D:\workspace\github\openSA\book\src\introduction.md",
    "D:\workspace\github\openSA\book\src\99-appendix\contributing.md"
)
foreach ($f in $files) {
    (Get-Content $f -Raw) -replace $old, $new | Set-Content $f -NoNewline
}
```

预期：4 个文件中的 `<owner>` 全部被替换。

- [ ] **Step 4: 提交并推送占位符替换**

```powershell
git add README.md book/book.toml book/src/introduction.md book/src/99-appendix/contributing.md
git commit -m "chore: replace <owner> placeholder with actual GitHub username"
git push
```

- [ ] **Step 5: 配置 GitHub Pages**

提示用户：

1. 在 GitHub 仓库页面点击 **Settings** → 左侧 **Pages**
2. **Source** 选择 **GitHub Actions**
3. 保存

- [ ] **Step 6: 验证部署**

```powershell
# 触发一次空 commit 确保 workflow 跑过
git commit --allow-empty -m "ci: trigger initial deploy"
git push
```

预期：在 GitHub 仓库 **Actions** 标签页看到 `Deploy mdBook site to Pages` workflow 跑过；访问 `https://<owner>.github.io/openSA/` 看到 mdBook 首页。

---

## Task 12: v0.1 验收

按设计文档第 12 节"验收标准"逐项核对：

- [ ] **Step 1: 本地 mdbook serve 验证**

```powershell
Set-Location D:\workspace\github\openSA\book
mdbook build
mdbook serve --port 3000
```

预期：build 0 错误 0 警告（warn 是允许的）；浏览器访问 `http://localhost:3000` 看到中文 mdBook 首页。**完成后 Ctrl+C**。

- [ ] **Step 2: 链接完整性检查**

```powershell
mdbook build
Get-ChildItem book\book\*.html | Measure-Object
```

预期：html 文件数 ≥ 10（每个章节至少 1 个）。

- [ ] **Step 3: GitHub 部署验证**

打开 https://\<owner\>.github.io/openSA/ ，确认：
- [ ] 标题是 `openSA · 开源系统架构师`
- [ ] 默认主题为浅色，可切换深色
- [ ] 左侧导航有 4 大模块（基础能力 / 进阶能力 / 案例库 / 附录）
- [ ] 点击任一章节不出现 404
- [ ] 顶部 "GitHub" 图标可跳到仓库

- [ ] **Step 4: 提交验收记录**

```powershell
# 在 README 底部"路线图"区，把 v0.1 标记为已完成（可选）
# 不强制要求 commit
```

---

## 附录 A：常见问题排查

### A.1 mdbook build 报"missing chapter"

- 原因：SUMMARY.md 引用的文件未创建
- 解决：检查报错信息，对应文件补齐

### A.2 cargo install mdbook 编译失败

- 原因：通常是网络问题或 Rust 版本过低
- 解决：用 `rustup update stable` 后重试

### A.3 GitHub Actions 部署失败

- 排查：进入仓库 Actions 标签 → 点击失败 run → 看日志
- 常见原因：Pages 源未选 GitHub Actions（参见 Task 11 Step 5）

### A.4 推送到 GitHub 后 Pages 仍是 404

- 排查：Settings → Pages 确认 source 正确
- 排查：Actions 标签确认 deploy job 通过
- 排查：URL 拼写（`https://<owner>.github.io/openSA/`）

---

## 附录 B：明确不做的（YAGNI）

- ❌ 不写完整章节内容（v0.1 阶段）
- ❌ 不做自定义主题
- ❌ 不做站内搜索
- ❌ 不做评论 / 反馈系统
- ❌ 不做 i18n（仅中文）
- ❌ 不引入其他静态站点生成器对比
- ❌ 不做 monorepo / workspace（无 Rust 代码）

---

## 附录 C：执行模式选择

**完成本计划有两种执行方式**：

1. **Subagent-Driven**（推荐）：每个 Task 派一个独立 subagent 执行，主会话做两阶段 review。速度快、隔离好。
2. **Inline Execution**：在当前会话里按顺序执行 Task，批处理 + 检查点。简单直接。

**用户选择后将切换到对应子技能**（`superpowers:subagent-driven-development` 或 `superpowers:executing-plans`）。
