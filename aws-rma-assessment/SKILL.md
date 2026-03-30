---
name: rma-assessment-assistant
description: >-
  Intelligent RMA (Reliability, Maintainability, Availability) Resilience Assessment Assistant.
  Conducts interactive Q&A based on AWS Well-Architected Framework to evaluate application
  resilience maturity, automatically generating assessment reports and improvement roadmaps.
  Supports compact version (36 core questions) and full version (80 questions).
  Use this skill when users want to assess application resilience maturity, run an RMA
  questionnaire, evaluate reliability readiness, or benchmark against AWS Well-Architected
  best practices — even if they just say "check how resilient my app is" or "韧性评估"
  or "成熟度评估".
allowed-tools: Read, Write, Grep, Glob, AskUserQuestion
model: sonnet
---

# Language / 语言

- If the user speaks English, follow [SKILL_EN.md](SKILL_EN.md)
- 如果用户使用中文，请遵循 [SKILL_ZH.md](SKILL_ZH.md)

Detect the language from the user's message and load the corresponding instruction file.
