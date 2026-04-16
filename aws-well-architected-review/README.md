**English** | [中文](README_zh.md)

---

# AWS Well-Architected Review Skill

An automated AWS Well-Architected Framework Review skill that programmatically assesses your AWS environment across all 6 WAF pillars using read-only API calls.

## Installation

**Option A: npx skills (Recommended)**
```bash
npx skills add aws-samples/sample-aws-resilience-skill --skill aws-well-architected-review
```

**Option B: Git clone**
```bash
git clone https://github.com/aws-samples/sample-aws-resilience-skill.git
```

## Features

- ✅ **6-Pillar Assessment**: Security, Operational Excellence, Reliability, Performance, Cost Optimization, Sustainability
- ✅ **Security-First**: Security pillar always assessed first as the foundation
- ✅ **49 Programmatic Checks**: All using read-only AWS CLI commands (Describe/Get/List only)
- ✅ **Autopilot Mode**: Minimal human interaction — confirm credentials, then fully automated
- ✅ **Risk Classification**: HRI/MRI/LRI severity-based risk portfolio
- ✅ **Dual Reports**: Markdown + HTML with pillar scorecards and improvement roadmap
- ✅ **Credential Safety**: Enforces read-only permission boundary — blocks write-capable credentials
- ✅ **WA Tool Sync**: Optional sync findings to AWS WA Tool console

## How It Works

```
Phase 1: Bootstrap (~2 min)     → Credential validation + scope confirmation
Phase 2: Discover  (~15-30 min) → 6-pillar programmatic scan
Phase 3: Analysis  (~5 min)     → Risk identification + prioritization
Phase 4: Report    (~2 min)     → Markdown + HTML report generation
```

## Pillar Checks Summary

| Pillar | Checks | Key Areas |
|--------|--------|-----------|
| 🔒 Security | 12 | GuardDuty, Security Hub, CloudTrail, IAM, encryption, network |
| ⚙️ Ops Excellence | 8 | AWS Config, CloudWatch, patching, CFN health |
| 🔄 Reliability | 9 | Multi-AZ, backups, ASG, health checks, PITR |
| ⚡ Performance | 7 | Instance types, EBS, Compute Optimizer |
| 💰 Cost | 8 | Idle resources, Savings Plans, lifecycle policies |
| 🌱 Sustainability | 5 | Graviton adoption, utilization, right-sizing |

## Quick Start

1. Ensure AWS CLI is configured with read-only credentials
2. Say **"Start WA Review"** or **"开始架构评审"** to your AI assistant
3. Confirm target account, region, and scope
4. Wait ~20-30 minutes for the automated assessment
5. Review the generated report in `wafr-reports/`

## Prerequisites

- AWS CLI v2 installed and configured
- IAM role/user with `ReadOnlyAccess` or equivalent read-only policy
- ~30 minutes for a full 6-pillar assessment

## Output Example

```
Overall Health: 2.7/5 ★★★☆☆

| Pillar              | Score | CRITICAL | HIGH | MEDIUM | LOW |
|---------------------|-------|----------|------|--------|-----|
| 🔒 Security         | 2/5   | 0        | 4    | 3      | 0   |
| ⚙️ Ops Excellence   | 3/5   | 0        | 1    | 0      | 0   |
| 🔄 Reliability      | 2/5   | 0        | 3    | 0      | 0   |
| ⚡ Performance       | 4/5   | 0        | 0    | 1      | 0   |
| 💰 Cost             | 4/5   | 0        | 0    | 1      | 0   |
| 🌱 Sustainability   | 4/5   | 0        | 0    | 0      | 1   |
```

## Integration with Other Skills

| Skill | Integration |
|-------|------------|
| [aws-resilience-modeling](../aws-resilience-modeling/) | Deep-dive reliability analysis on HRI findings |
| [chaos-engineering-on-aws](../chaos-engineering-on-aws/) | Generate chaos test plans from risk inventory |
| [aws-rma-assessment](../aws-rma-assessment/) | Organizational maturity scoring |

## License

This project is licensed under the MIT-0 License. See the [LICENSE](../LICENSE) file.
