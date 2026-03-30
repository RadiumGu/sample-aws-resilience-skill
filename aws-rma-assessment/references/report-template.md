# RMA Assessment Report Template

> This file contains the complete report template structure and HTML generation guide for RMA assessments.
> See [SKILL_EN.md](../SKILL_EN.md) Steps 6 and 7 for the main workflow overview.

---

## Report Filename Format

`{application-name}-rma-assessment-{date}.md`

---

## Report Structure

```markdown
# RMA Resilience Assessment Report
## {Application Name}

**Assessment Date**: {date}
**Assessment Version**: Compact / Full
**Overall Maturity**: {score}% - {rating}

---

## Executive Summary

### Overall Assessment
- Total questions: {count}
- Average maturity level: {level}
- Overall score: {score}% - {rating}
- **AI-assisted efficiency**: Saved {percentage}% time, auto-analyzed {count} questions

### Maturity Radar Chart

Use Mermaid charts to display maturity across 10 domains:

\`\`\`mermaid
---
config:
  themeVariables:
    xyChart:
      plotColorPalette: "#2563eb"
---
%%{init: {'theme':'base'}}%%
graph TD
    subgraph Resilience Maturity Radar
        A[Recovery Objectives: 85%]
        B[Observability: 72%]
        C[Disaster Recovery: 65%]
        D[High Availability: 78%]
        E[Change Management: 88%]
        F[Incident Management: 70%]
        G[Operations Reviews: 60%]
        H[Chaos Engineering: 45%]
        I[Game Days: 40%]
        J[Organizational Learning: 55%]
    end
\`\`\`

**Or use table format:**

| Domain | Score | Rating | Trend |
|--------|-------|--------|-------|
| Recovery Objectives | 85% | Good | 🟢 |
| Observability | 72% | Fair | 🟡 |
| Disaster Recovery | 65% | Fair | 🟡 |
| High Availability | 78% | Good | 🟢 |
| Change Management | 88% | Good | 🟢 |
| Incident Management | 70% | Fair | 🟡 |
| Operations Reviews | 60% | Fair | 🟡 |
| Chaos Engineering | 45% | Needs Improvement | 🔴 |
| Game Days | 40% | Critical | 🔴 |
| Organizational Learning | 55% | Needs Improvement | 🟡 |

### Gap Heatmap

**Gap distribution by priority and domain:**

| Domain | P0 Gaps | P1 Gaps | P2 Gaps | P3 Gaps | Total |
|--------|---------|---------|---------|---------|-------|
| Recovery Objectives | 🟢 0 | 🟢 0 | 🟡 1 | - | 1 |
| Observability | - | 🟡 2 | 🟡 3 | - | 5 |
| Disaster Recovery | 🔴 2 | 🟡 1 | - | 🟢 0 | 3 |
| High Availability | 🟡 1 | 🟢 0 | - | - | 1 |
| Change Management | 🟢 0 | 🟢 0 | 🟡 1 | - | 1 |
| Incident Management | 🟡 1 | 🟡 2 | 🟢 0 | - | 3 |
| Chaos Engineering | - | - | 🔴 5 | 🔴 3 | 8 |
| Game Days | - | - | - | 🔴 3 | 3 |

**Legend:**
- 🔴 Gaps >= 3 questions (Critical)
- 🟡 Gaps 1-2 questions (Moderate)
- 🟢 Gaps 0 questions (Good)

### Top 5 Key Findings

1. **[Domain] - Question X**: {description}
   - Current state: Level {1/2/3}
   - Risk level: High/Medium/Low
   - Business impact: {description}
   - **AI analysis basis**: {auto-analysis source}

### Strength Areas

List domains scoring Level 3, noting whether auto-identified

---

## Domain Assessment Details

### 1. Recovery Objectives
**Domain Score**: {score}% - {rating}

| Q ID | Question | Current Level | Target Level | Gap |
|------|----------|---------------|--------------|-----|
| 1 | ... | 2 | 3 | Needs Improvement |

**Domain Analysis:**
{Analysis and recommendations based on answers}

### 2. Observability
...

{Repeat for all domains}

---

## Improvement Roadmap

### Phase 1 (0-3 months): Critical Risk Mitigation

**Priority**: P0

| Q ID | Improvement Item | Current->Target | AWS Service Recommendation | Est. Effort | Est. Cost |
|------|-----------------|-----------------|---------------------------|-------------|-----------|
| 27 | Implement DR strategy | 1->3 | Aurora Global DB, Route 53 | 2-3 weeks | +$1500/mo |

### Phase 2 (3-6 months): Important Improvements

**Priority**: P1

{Similar format}

### Phase 3 (6-12 months): Maturity Uplift

**Priority**: P2 + P3

{Similar format}

---

## AWS Service Recommendations

Based on gap analysis, the following AWS services are recommended:

| Service | Purpose | Addresses | Est. Monthly Cost |
|---------|---------|-----------|-------------------|
| AWS Resilience Hub | Automated resilience assessment | Q38 | $0 (per-assessment billing) |
| AWS FIS | Chaos engineering testing | Q62-68 | ~$100/mo |
| CloudWatch Synthetics | Synthetic monitoring | Q17 | ~$50/mo |

---

## Detailed Q&A Records

### P0 - Critical Questions

#### Question 1: How do you define recovery objectives for your application?
- **Answer**: {user's answer}
- **Maturity Level**: {1/2/3}
- **Assessment Basis**: {rationale for the level}
- **Improvement Recommendation**: {specific advice if not Level 3}

{Detailed records for all questions}

---

## Reference Resources

- [AWS Well-Architected Framework - Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/)
- [AWS Resilience Hub](https://aws.amazon.com/resilience-hub/)
- [AWS Fault Injection Simulator](https://aws.amazon.com/fis/)

---

**Report generated**: {datetime}
**Assessment tool**: RMA Assessment Assistant v1.0
```

---

## HTML Report Generation (Optional)

If the user needs an HTML version, use pandoc to convert:

```bash
# Check if pandoc is available
if command -v pandoc &> /dev/null; then
    pandoc {report-file}.md \
      -f gfm \
      -t html5 \
      --standalone \
      --toc \
      --toc-depth=3 \
      --css=https://cdn.jsdelivr.net/npm/github-markdown-css@5/github-markdown.min.css \
      --metadata title="RMA Resilience Assessment Report" \
      -o {report-file}.html
fi
```
