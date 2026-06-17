# 代码仓库配置方案总览

这份方案的核心不是把代码放到 GitHub 上，而是建立一套可长期运行的工程制度。仓库应该同时承担以下职责：

- 代码协作入口
- 架构和知识沉淀入口
- CI/CD 自动化入口
- 安全和合规检查入口
- 发布、回滚和审计入口

## 一句话结论

大多数团队默认选 **GitHub**；强合规、私有化、内网场景默认选 **GitLab**；已有 Jira / Confluence 深度协作体系时考虑 **Bitbucket**；预算敏感、轻量自建时考虑 **Gitea**。

## 推荐落地组合

| 层级 | 推荐做法 |
|---|---|
| 平台 | GitHub Team / Enterprise Cloud，私有化则 GitLab Self-Managed |
| 分支 | Trunk-Based Development + 短功能分支 |
| 提交 | Conventional Commits |
| 审查 | PR + CODEOWNERS + 必需审批 |
| CI/CD | 测试、构建、扫描、发布、回滚自动化 |
| 安全 | Secret 扫描、依赖漏洞、SAST、许可证策略 |
| 发布 | SemVer + CHANGELOG + 自动 tag / Release |
| 治理 | 模板仓库、规则集、权限分层、Onboarding 文档 |

## 仓库应包含什么

```text
repo-root/
├─ apps/                  # 可部署应用
├─ libs/                  # 共享库
├─ deploy/                # 部署清单
├─ docs/                  # 架构、ADR、Runbook、Onboarding
├─ scripts/               # 工程自动化脚本
├─ tests/                 # 集成测试和 E2E
├─ .github/               # GitHub 工作流和模板
├─ CODEOWNERS
├─ README.md
├─ CONTRIBUTING.md
├─ SECURITY.md
├─ CHANGELOG.md
└─ Makefile
```

## 最小可行版本

如果团队只有几个人，不要一上来就搞“企业级平台治理”，那通常会把仓库变成一座没人敢碰的神庙。先做这 6 件事：

1. 主分支保护
2. PR 审查
3. 基础 CI
4. README + CONTRIBUTING
5. Conventional Commits
6. Secret 扫描

## 成熟版本

团队扩大后，再补齐：

- CODEOWNERS
- 组织级 rulesets
- merge queue / merge trains
- 自动版本发布
- 受保护环境
- DORA 指标
- 恢复演练
