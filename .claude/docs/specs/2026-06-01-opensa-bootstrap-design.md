# openSA 项目骨架设计规范

- **日期**：2026-06-01
- **作者**：openSA 启动会话
- **状态**：已通过用户审阅，待实施
- **范围**：v0.1 骨架（MVP）

---

## 1. 项目目标

**openSA**（Open System Architect）是一个面向**初中级工程师到架构师**的中文架构师知识库与真实案例库。

| 维度 | 决策 |
|---|---|
| 定位 | 知识库 + 真实案例库 |
| 读者 | 初中级 → 架构师 全覆盖 |
| 语言 | 中文为主（技术术语保留英文） |
| 范围 | 岗位角色视角（设计 / 决策 / 沟通 / 工程 / 领导） |
| 部署 | GitHub Pages，gh-pages 分支，mdbook-action 风格 workflow |

---

## 2. 技术选型

- **静态站点生成器**：[mdBook](https://rust-lang.github.io/mdBook/)（Rust 实现）
- **托管**：GitHub Pages
- **CI/CD**：GitHub Actions（`actions/checkout@v4` + `taiki-e/install-action` 装 mdbook + `actions/deploy-pages@v4`）
- **许可证**：MIT
- **版本控制**：git，主分支 `main`

---

## 3. 仓库根目录结构

```
openSA/
├── .claude/                          # AI 辅助与项目记忆
│   └── docs/                         # AI 生成的辅助文档（不进入 mdBook）
│       ├── specs/                    # 设计规范
│       └── notes/                    # 工作笔记
├── .github/
│   └── workflows/
│       └── mdbook.yml                # 构建 + 部署到 gh-pages
├── book/                             # mdBook 项目根
│   ├── book.toml                     # mdBook 配置
│   └── src/                          # Markdown 源文件
│       ├── SUMMARY.md                # 章节目录
│       ├── introduction.md           # 引言
│       ├── 01-design/README.md       # 设计能力
│       ├── 02-decision/README.md     # 决策能力
│       ├── 03-communication/README.md# 沟通能力
│       ├── 04-engineering/README.md  # 工程能力
│       ├── 05-leadership/README.md   # 领导能力
│       ├── 06-integration/README.md  # 跨域整合
│       ├── 07-complexity/README.md   # 复杂系统驾驭
│       ├── 20-cases/                 # 案例库（独立 section）
│       │   ├── README.md
│       │   ├── by-industry/README.md
│       │   ├── by-scale/README.md
│       │   ├── by-topic/README.md
│       │   └── TEMPLATE.md           # 案例贡献模板
│       └── 99-appendix/              # 附录
│           ├── glossary.md
│           ├── reading-paths.md
│           └── contributing.md
├── assets/                           # 全局静态资源（图片、PDF）
├── README.md                         # GitHub 仓库门面
├── CONTRIBUTING.md                   # 贡献指南
├── LICENSE                           # MIT
└── .gitignore
```

**关键设计决策**：
1. `book/src/` 命名空间：mdBook 约定源文件在 `src/`
2. 章节用数字前缀（`01-`、`02-` …）保证 SUMMARY.md 排序稳定
3. 案例库用 `20-` 前缀放在主目录之后，暗示"延伸阅读"
4. `.claude/docs/` 与 mdBook 源物理隔离，AI 工作产物不污染主站

---

## 4. mdBook 配置

`book/book.toml`：

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

> **说明**：`<owner>` 在 git remote 配置完成后回填。

---

## 5. SUMMARY.md（最小骨架）

```markdown
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
```

**渐进式填充策略**：
- v0.1：每个 README.md / 占位文件都放"待编写"引导文字
- v0.2+：按 sprint 计划逐步填充内容
- SUMMARY.md 中已占位但暂未细化的章节 → 留作后续扩展点

---

## 6. README.md（GitHub 仓库门面）

```markdown
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

\`\`\`bash
cargo install mdbook
git clone https://github.com/<owner>/openSA.git
cd openSA/book
mdbook serve --open
\`\`\`

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
```

---

## 7. GitHub Actions 部署

`.github/workflows/mdbook.yml`：

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

**Pages 设置说明**（README 不写，部署后由维护者在仓库 Settings → Pages 选定 "GitHub Actions" 作为 source）。

---

## 8. 占位文件统一模板

除 `SUMMARY.md` 之外的所有章节 .md 文件，骨架阶段内容如下：

```markdown
# <章节名>

> 本章节待编写。openSA 欢迎贡献者补充。
>
> **建议覆盖**：
> - 主题 1
> - 主题 2
> - 主题 3
>
> 参见 [贡献指南](../99-appendix/contributing.md)。
```

`20-cases/TEMPLATE.md`（案例库专用）：

```markdown
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
| | | |

## 结果与反思

效果如何？回头看有何得失？

## 参考资料

（可选）
```

---

## 9. .gitignore

```
# mdBook 构建产物
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
*.bak
```

---

## 10. CONTRIBUTING.md 骨架

```markdown
# 贡献指南

## 提交 Issue

- 选题建议
- 错误反馈（请附 URL 与上下文）
- 内容讨论

## 提交 PR

1. Fork → 新分支 → 修改
2. 本地 `cd book && mdbook serve` 自检
3. PR 描述：动机 / 改动点 / 关联 Issue
4. 案例库贡献必须使用 [TEMPLATE.md](./book/src/20-cases/TEMPLATE.md)

## 内容规范

- 中文为主，技术术语可保留英文
- 每章首部加"读者分级"标签（入门 / 进阶 / 架构师）
- 引用外部资料附链接
- 配图放在 `assets/`，引用用相对路径

## 行为准则

欢迎、尊重、专注内容本身。
```

---

## 11. 实施步骤（带验收）

| 步骤 | 动作 | 验证 |
|---|---|---|
| 1 | 安装并验证 Rust 工具链与 mdbook | `cargo --version` / `mdbook --version` |
| 2 | `git init` → 提交 LICENSE / .gitignore / README / CONTRIBUTING | `git status` 干净；`.claude/docs/` 本次**不**纳入 git 跟踪（AI 工作产物与项目主内容物理隔离） |
| 3 | 创建 `book/src/` 骨架与 SUMMARY.md | `cd book && mdbook build` 无错 |
| 4 | 添加 `.github/workflows/mdbook.yml` | 文件存在，YAML 语法合法 |
| 5 | 占位 .md 文件按统一模板填充 | 每个 `book/src/**/*.md` 存在且有引导文字 |
| 6 | 首次 commit + 设置 remote + push | `git log` 有 commit；GitHub 仓库可见 |
| 7 | 仓库 Settings → Pages 选 GitHub Actions | （首次部署后由维护者配置） |
| 8 | 验证 Pages 部署 | https://\<owner\>.github.io/openSA/ 可访问 |

---

## 12. 验收标准（v0.1 DoD）

1. ✅ `git clone` → `cd book && mdbook serve` 可起站，无 404
2. ✅ push 到 main → GitHub Actions 跑过 → gh-pages 部署成功
3. ✅ README.md 在 GitHub 仓库首页渲染正常（中文、表格、徽章位等）
4. ✅ 7 大能力章节 + 案例库 + 附录占位齐全
5. ✅ LICENSE = MIT，含版权年份与作者占位
6. ✅ CONTRIBUTING.md 至少 100 字

---

## 13. 明确不做（YAGNI）

- ❌ 不写完整章节内容（v0.1 阶段）
- ❌ 不做自定义主题
- ❌ 不做站内搜索
- ❌ 不做评论 / 反馈系统
- ❌ 不做 i18n（仅中文）
- ❌ 不引入其他静态站点生成器对比
- ❌ 不做 monorepo / workspace（无 Rust 代码）

---

## 14. 待回填的占位符

实施时需要替换：
- `<owner>` → GitHub 用户名或组织名
- LICENSE 中的版权年份与持有人
- README 与 GitHub 用户名联动处
