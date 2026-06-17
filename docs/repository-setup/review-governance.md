# 审查、权限与协作治理

代码审查的目标不是让大家互相找茬，而是让变更风险可见、知识流动、责任明确。虽然很多团队最后会把 PR 变成“求求你点个 Approve”的仪式，但我们还是可以挣扎一下。

## 推荐审查基线

| 维度 | 建议 |
|---|---|
| 主分支直接推送 | 禁止 |
| 变更入口 | 全部通过 PR / MR |
| 必需审批数 | 小团队 1 个，中大型团队 2 个 |
| CODEOWNERS | 必开 |
| CI 状态检查 | 必须通过 |
| 线程解决 | 必须解决 |
| 新 push 后旧审批 | 建议自动失效 |
| 自批准 | 不计入 |

## PR 模板

```md
## 背景
<!-- 为什么要做这次改动 -->

## 改动摘要
<!-- 改了什么 -->

## 测试
- [ ] 单元测试
- [ ] 集成测试
- [ ] 手工验证

## 风险
- 影响范围：
- 兼容性影响：
- 是否涉及数据库/配置变更：

## 回滚
<!-- 如何快速回滚 -->

## Checklist
- [ ] 提交信息符合规范
- [ ] 已更新文档
- [ ] 未引入密钥
- [ ] 已添加/更新测试
```

## CODEOWNERS 示例

```text
*                           @org/platform-team
/apps/admin-web/            @org/web-team
/apps/api-gateway/          @org/backend-team
/apps/worker-billing/       @org/billing-team
/libs/domain-user/          @org/arch-team @org/backend-team
/deploy/                    @org/sre-team
/.github/                   @org/platform-team
/docs/runbooks/             @org/sre-team
```

## 权限模型

| 角色 | 权限 | 职责 |
|---|---|---|
| 平台管理员 | Admin | 规则集、Runner、模板、组织级策略 |
| 仓库维护者 | Maintain / Maintainer | 分支规则、发布、紧急回滚 |
| 开发者 | Write / Developer | 分支开发、PR、非生产部署 |
| Reviewer / Owner | Review 权限 | 代码审查、CODEOWNERS 审批 |
| 安全/合规 | 审批与读取 | 敏感路径、生产审批、审计 |

## 分支保护建议

GitHub：

- Require pull request before merging
- Require approvals
- Require review from Code Owners
- Dismiss stale approvals
- Require status checks
- Require branches to be up to date
- Restrict who can push
- Enable merge queue for high并发仓库

GitLab：

- Protected branches
- Merge request approvals
- Code Owner approvals
- Protected environments
- Deployment approvals
- Merge trains

## RACI 示例

| 事项 | 平台团队 | 技术负责人 | 服务 Owner | 开发者 | 安全/合规 |
|---|---|---|---|---|---|
| 模板仓库维护 | A/R | C | I | I | C |
| 分支/规则配置 | R | C | A | I | C |
| 日常代码评审 | I | C | A/R | R | I |
| 发布审批 | I | A | R | C | C |
| 安全漏洞修复 SLA | C | A | R | R | A/R |
| Onboarding 文档 | C | A | R | I | I |

## 协作治理原则

1. 规则要写进仓库，不要靠口头传承。
2. 风险路径必须有明确 owner。
3. CI 失败不允许“先合了再说”。
4. 紧急修复可以走快速流程，但必须补审计记录。
5. 每个服务都要有 README、Runbook、回滚说明。
