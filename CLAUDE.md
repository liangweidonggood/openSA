# openSA 项目约定

## 环境

- **shell 是 Git Bash**（Windows 11 上），路径用 POSIX 风格（`/d/workspace/github/openSA`）
- **gh CLI 已认证**：账户 `liangweidonggood`，token 权限含 `repo` / `workflow` / `admin:org`，可跳过认证步骤
- **Rust 工具链**已就绪（`~/.cargo/bin`）
- **mdbook v0.5+** 已安装

## 工作流提示

- **mdbook 源在 `book/src/`，产物在 `book/book/`**（被 .gitignore 忽略）
- **mdbook v0.5 对 `create-missing=true` 鲁棒**：不会报"missing chapter" 错误，会生成占位页
- **GitHub Pages 首次 deploy 必失败**（`actions/deploy-pages@v4` 报 404）：
  1. 先 `gh api -X POST repos/liangweidonggood/openSA/pages -f build_type=workflow` 启用
  2. 再 push 或推空 commit `git commit --allow-empty -m "ci: trigger Pages deployment"` 重跑
- **Write 工具**：对**已存在**文件需先 Read 再 Write；新文件可直接 Write；兜底用 `sed -i` 或 `Set-Content`
- **CRLF 警告**：Windows + Git Bash 下 `git commit` 总是有 `LF will be replaced by CRLF` 警告，可忽略
- **站点**：https://liangweidonggood.github.io/openSA/  ｜ **仓库**：https://github.com/liangweidonggood/openSA

## 设计/计划文档

- 设计规范：`.claude/docs/specs/`
- 实施计划：`.claude/docs/plans/`
- AI 工作笔记：`.claude/docs/notes/`（默认不入 git）

## 路线图（README 已声明）

- v0.1 ✅ 骨架 + 部署通路
- v0.2 基础能力章节首批内容（设计能力 3 章）
- v0.3 案例库首批 5 个真实案例
- v0.4 进阶能力章节
- v1.0 内容覆盖完整 / 中级成熟度
