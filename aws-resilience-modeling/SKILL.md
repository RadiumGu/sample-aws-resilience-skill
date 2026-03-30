---
name: aws-resilience-modeling
description: >-
  Conduct comprehensive AWS system resilience analysis and risk assessment.
  Use when the user wants to evaluate AWS infrastructure resilience, identify
  failure modes, assess system reliability, or create disaster recovery plans.
  Also use when users ask about system availability, failure risks, reliability
  improvement, or how their AWS setup handles outages — even if they don't
  explicitly say "resilience". Automatically invoked for AWS韧性分析, 系统风险评估,
  AWS弹性评估, 可靠性评估, 灾难恢复规划, 故障模式分析.
allowed-tools: Bash(aws *), Bash(gh *), Read, Write, Grep, Glob, Task
model: sonnet
---

# Language / 语言

- If the user speaks English, follow [SKILL_EN.md](SKILL_EN.md)
- 如果用户使用中文，请遵循 [SKILL_ZH.md](SKILL_ZH.md)

Detect the language from the user's message and load the corresponding instruction file.
