# 配置一个好的代码仓库

这套文档把“建一个 Git 仓库”升级成“建立一个可治理、可扩展、可审计、可持续交付的工程系统”。

它适合以下场景：

- 新团队要搭建标准代码仓库
- 现有仓库混乱，需要治理
- 准备接入 CI/CD、安全扫描、发布和回滚流程
- 想把 GitHub / GitLab 当成真正的工程中枢，而不是文件夹收藏夹

## 默认推荐

| 场景 | 推荐方案 |
|---|---|
| 大多数云优先团队 | GitHub + GitHub Actions + GHCR + CODEOWNERS + Rulesets |
| 合规、私有化、内网团队 | GitLab Self-Managed + GitLab CI/CD + Protected Environments |
| Atlassian 深度用户 | Bitbucket + Jira + Pipelines |
| 轻量自建 | Gitea |

## 推荐基线

- 使用 Trunk-Based Development + 短生命周期功能分支
- 所有变更通过 PR/MR
- 主分支禁止直接推送
- 使用 Conventional Commits
- 引入 CODEOWNERS 和必需审批
- CI 至少覆盖 lint、test、build、安全扫描
- 发布采用 SemVer + CHANGELOG + tag
- 安全侧启用 Secret 扫描、依赖漏洞扫描、许可证策略

## 快速开始

```bash
pip install -r requirements.txt
mkdocs serve
```

本地访问：

```text
http://127.0.0.1:8000
```

## 阅读顺序

1. 先读 [总览](repository-setup/index.md)
2. 再读 [平台选型](repository-setup/platform-selection.md)
3. 如果你要直接落地，跳到 [落地计划与模板](repository-setup/rollout-templates.md)
