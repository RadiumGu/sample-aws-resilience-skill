[**中文**](README_zh.md) | English

---

# AWS Well-Architected 自动化评审 Skill

一个自动化的 AWS Well-Architected Framework 评审 Skill，通过只读 API 调用对 AWS 环境进行 6 大 WAF 支柱的编程式评估。

## 安装

**方式 A: npx skills（推荐）**
```bash
npx skills add aws-samples/sample-aws-resilience-skill --skill aws-well-architected-review
```

**方式 B: Git clone**
```bash
git clone https://github.com/aws-samples/sample-aws-resilience-skill.git
```

## 特性

- ✅ **6 大支柱全覆盖**：安全、卓越运营、可靠性、性能效率、成本优化、可持续性
- ✅ **Security-First**：安全支柱始终第一个评估，作为其他支柱的基础
- ✅ **49 项编程式检查**：全部使用只读 AWS CLI 命令（仅 Describe/Get/List）
- ✅ **自动驾驶模式**：确认凭证后全自动执行，无需人工干预
- ✅ **风险分级**：HRI（高风险）/ MRI（中风险）/ LRI（低风险）严重性分级
- ✅ **双格式报告**：Markdown + HTML，含支柱计分卡和改进路线图
- ✅ **凭证安全**：强制只读权限边界——阻止有写权限的凭证
- ✅ **WA Tool 同步**：可选将评估结果同步到 AWS WA Tool 控制台

## 工作流程

```
阶段 1: 环境引导 (~2 分钟)      → 凭证验证 + 范围确认
阶段 2: 发现扫描 (~15-30 分钟)  → 6 支柱编程式检查
阶段 3: 风险分析 (~5 分钟)      → 风险识别 + 优先级排序
阶段 4: 报告生成 (~2 分钟)      → Markdown + HTML 报告
```

## 各支柱检查项

| 支柱 | 检查数 | 关键领域 |
|------|--------|---------|
| 🔒 安全 | 12 | GuardDuty、Security Hub、CloudTrail、IAM、加密、网络 |
| ⚙️ 卓越运营 | 8 | AWS Config、CloudWatch、补丁管理、CFN 健康 |
| 🔄 可靠性 | 9 | Multi-AZ、备份、ASG、健康检查、PITR |
| ⚡ 性能效率 | 7 | 实例类型、EBS、Compute Optimizer |
| 💰 成本优化 | 8 | 闲置资源、Savings Plans、生命周期策略 |
| 🌱 可持续性 | 5 | Graviton 采用率、利用率、Right-sizing |

## 快速开始

1. 确保 AWS CLI 已配置只读凭证
2. 对 AI 助手说 **"开始架构评审"** 或 **"Start WA Review"**
3. 确认目标账户、Region 和评估范围
4. 等待约 20-30 分钟完成自动评估
5. 查看 `wafr-reports/` 目录中的报告

## 前置条件

- AWS CLI v2 已安装并配置
- 具有 `ReadOnlyAccess` 或等效只读策略的 IAM 角色/用户
- 完整 6 支柱评估约需 30 分钟

## 输出示例

```
总体健康度: 2.7/5 ★★★☆☆

| 支柱              | 分数 | CRITICAL | HIGH | MEDIUM | LOW |
|-------------------|------|----------|------|--------|-----|
| 🔒 安全           | 2/5  | 0        | 4    | 3      | 0   |
| ⚙️ 卓越运营       | 3/5  | 0        | 1    | 0      | 0   |
| 🔄 可靠性         | 2/5  | 0        | 3    | 0      | 0   |
| ⚡ 性能效率        | 4/5  | 0        | 0    | 1      | 0   |
| 💰 成本优化        | 4/5  | 0        | 0    | 1      | 0   |
| 🌱 可持续性        | 4/5  | 0        | 0    | 0      | 1   |
```

## 与其他 Skill 集成

| Skill | 集成方式 |
|-------|---------|
| [aws-resilience-modeling](../aws-resilience-modeling/) | 对 HRI 发现做深度可靠性分析 |
| [chaos-engineering-on-aws](../chaos-engineering-on-aws/) | 从风险清单生成混沌工程测试计划 |
| [aws-rma-assessment](../aws-rma-assessment/) | 组织级韧性成熟度评估 |

## 许可证

本项目采用 MIT-0 许可证。详见 [LICENSE](../LICENSE) 文件。
