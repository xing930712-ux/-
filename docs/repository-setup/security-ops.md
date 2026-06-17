# 安全、合规、运维与监控

安全基线的核心是：左移、分层、最小权限。也就是说，别等密钥已经躺在 Git 历史里三个月后再开始表演慌张。

## 四层防护

| 阶段 | 目标 | 推荐措施 |
|---|---|---|
| 本地提交前 | 阻止低级错误进入仓库 | pre-commit、gitleaks、lint |
| PR 阶段 | 阻断风险变更 | dependency review、SAST、Secret scan |
| main 阶段 | 生成可信制品 | 镜像扫描、SBOM、许可证检查 |
| 发布阶段 | 控制生产风险 | OIDC、环境审批、最小权限、审计日志 |

## Secret 管理

原则：

- 不要把 Token、证书、私钥、数据库密码写进仓库
- `.env` 必须进 `.gitignore`
- 提供 `.env.example` 作为配置模板
- CI/CD 使用平台 Secret 管理能力
- 云访问优先用 OIDC，少用长期密钥

推荐 `.gitignore`：

```gitignore
.env
.env.*
!.env.example
*.pem
*.key
*.p12
```

## 依赖安全

推荐组合：

- Dependabot 或 Renovate：自动更新依赖
- Dependency Review：PR 阶段阻断高危依赖
- Trivy / OSV-Scanner：补充漏洞扫描
- 许可证策略：阻断不允许的 license

依赖治理不要只靠“没人会引入奇怪包吧”。这句话在人类软件史上从来没赢过。

## GitHub 安全基线

建议开启：

- Secret scanning
- Push protection
- Dependabot alerts
- Dependabot security updates
- Dependency review
- Code scanning
- Branch protection / Rulesets
- Environments with required reviewers

## GitLab 安全基线

建议开启：

- Secret Detection
- Dependency Scanning
- SAST
- Container Scanning
- License Compliance
- Protected Branches
- Protected Environments
- Deployment Approvals

## 运维监控指标

| 指标 | 说明 | 建议目标 |
|---|---|---|
| Pipeline 成功率 | 最近 7/30 天流水线通过率 | > 90% |
| PR 校验耗时 | 从 push 到 CI 结果完成 | < 10 分钟 |
| 主干构建耗时 | main 合并后到可部署 | < 15 分钟 |
| 缓存命中率 | 依赖和构建缓存效果 | > 60% |
| 部署频率 | 生产发布次数 | 越高越好 |
| MTTR | 故障平均恢复时间 | 越低越好 |
| 回滚成功率 | 回滚能否在 SLA 内完成 | > 95% |

## 备份与灾备

SaaS 平台不等于不需要备份。最低要求：

1. 代码仓库 mirror 备份
2. 重要 release tag 备份
3. CI/CD 配置备份
4. Secrets 清单备份，但不要明文备份密钥本体
5. 每季度恢复演练

示例：

```bash
git clone --mirror https://github.com/acme/repo.git backups/repo.git

git bundle create repo-$(date +%F).bundle --all
```

## 安全文档模板

```md
# Security Policy

## 支持版本

| 版本 | 是否支持 |
|---|---|
| latest | ✅ |
| N-1 | ✅ |
| 其他 | ❌ |

## 报告漏洞

请不要在公开 Issue 中披露安全漏洞。
请发送邮件到：security@example.com

## 响应 SLA

- 24 小时内确认收到
- 3 个工作日内给出初步评估
- 高危漏洞优先热修复
```
