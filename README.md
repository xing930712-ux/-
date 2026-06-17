# 配置一个好的代码仓库

这是一个工程治理与 DevOps 基线文档站，内容覆盖代码仓库选型、分支策略、代码审查、CI/CD、安全合规、发布回滚和团队落地方案。

## 在线文档

本仓库已配置 MkDocs + GitHub Pages 自动部署。推送到 `main` 后，GitHub Actions 会构建并发布文档站。

> 第一次使用时，请到仓库的 **Settings → Pages**，确认 Source 使用 **GitHub Actions**。也可以选择 **Deploy from a branch → gh-pages / root**。是的，连发布文档也要点一下按钮，人类文明的仪式感真顽强。

## 部署状态

Pages 设置已完成后，推送到 `main` 会自动触发文档站构建和发布。

## 部署方式

仓库已经配置两套发布方式：

1. `.github/workflows/deploy-docs.yml`：使用 GitHub Pages 官方 Actions 部署。
2. `.github/workflows/deploy-docs-branch.yml`：构建后推送到 `gh-pages` 分支，作为备用方式。

## 本地预览

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
pip install -r requirements.txt
mkdocs serve
```

浏览器打开：

```text
http://127.0.0.1:8000
```

## 文档结构

```text
docs/
  index.md
  repository-setup/
    index.md
    platform-selection.md
    repo-structure.md
    review-governance.md
    ci-cd.md
    security-ops.md
    rollout-templates.md
.github/workflows/deploy-docs.yml
.github/workflows/deploy-docs-branch.yml
mkdocs.yml
requirements.txt
```

## 推荐维护方式

- 文档变更使用 `docs:` 提交前缀
- 结构调整先更新 `mkdocs.yml` 导航
- 关键流程变更同步更新 CI/CD、安全和回滚章节
- 不要把敏感配置写进文档

## 常用命令

```bash
mkdocs serve      # 本地预览
mkdocs build      # 构建静态站点
mkdocs build --strict
```
