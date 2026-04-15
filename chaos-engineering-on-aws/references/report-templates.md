# Report Templates

> **Data Completeness Rules**: Before filling any template, check data availability per the
> Verdict Decision Tree in workflow-guide.md § 6.0.5. Fields marked 🔴 are mandatory —
> if data is unavailable, use the "Data Missing" replacement text shown below.

## Verdict Logic Reference

| Data Available | Verdict |
|---------------|---------|
| Complete metrics + baseline | PASSED ✅ or FAILED ❌ (based on hypothesis) |
| Metrics but no baseline | OBSERVED (baseline unknown) ⚠️ |
| No metrics data | OBSERVED (not validated) ⚠️ |
| No experiment result file | BLOCKED (no data) ❌ — do not generate report |

## Single Experiment Report

```markdown
# Chaos Engineering Experiment Report

## Experiment Overview
- Experiment name: {name}
- Risk ID: {risk_id} (from Assessment 2.4)
- Execution time: {timestamp}
- Environment: {env}
- Result: {PASSED ✅ / FAILED ❌ / ABORTED ⚠️ / OBSERVED ⚠️}  🔴 MANDATORY — use Verdict Decision Tree

## Input Source
- Assessment report: {filename}
- Risk description: {description}
- Assessment suggested injection method: {suggestion}
- Actual injection method: {actual}

## Steady-State Hypothesis vs. Actual Performance  🔴 MANDATORY

| Metric | Baseline | Hypothesis Threshold | During Experiment | After Recovery | Verdict |
|------|--------|---------|-----------|---------|------|
| Success rate | 99.98% | >= 99.5% | {value} | {value} | ✅/❌ |
| P99 latency | 120ms | <= 500ms | {value} | {value} | ✅/❌ |
| Recovery time | N/A | <= {RTO}s | {value} | N/A | ✅/❌ |

> **If metrics data missing**: Replace all `{value}` with "No data" (not "N/A (idle)").
> Add header: `⚠️ LIMITED DATA: No CloudWatch metrics were collected during this experiment.`

## Timeline
- T+0s: Fault injection started
- T+{X}s: Impact detected
- T+{Y}s: Recovery started
- T+{Z}s: Service fully recovered

## MTTR Phased Analysis

| Phase | Duration | Description | Improvement Direction |
|------|------|------|---------|
| Detection (fault → alarm) | {X}s | {description} | {suggestion} |
| Triage (alarm → root cause) | {X}s | {description} | {suggestion} |
| Response (root cause → fix) | {X}s | {description} | {suggestion} |
| Recovery (fix → restored) | {X}s | {description} | {suggestion} |
| **Total MTTR** | {X}s | | |

## Key Findings
1. ...

## Improvement Recommendations
1. **[P0]** ...
2. **[P1]** ...

## Resilience Score Update

| Dimension | Before Experiment (2.7) | After Experiment | Change |
|------|-------------|--------|------|
| Redundancy Design | ⭐ X/5 | ⭐ X/5 | — |
| ... | | | |

## Newly Discovered Risks

| Risk ID | Description | Severity | Recommendation |
|---------|------|--------|------|
| R-NEW-001 | ... | 🟠 High | ... |

## Cleanup Status

Post-experiment cleanup checklist. Check each item once completed.

### FIS Resources
- [ ] Experiment template deleted: `aws fis delete-experiment-template --id <TEMPLATE_ID>`
- [ ] Verify no FIS-created NACLs remain: `aws ec2 describe-network-acls --filters "Name=tag-key,Values=aws:fis:experiment-id"`
- [ ] Temporary stop-condition alarms deleted (if created only for this experiment): `aws cloudwatch delete-alarms --alarm-names <ALARM_NAMES>`

### Chaos Mesh Resources
- [ ] PodChaos / NetworkChaos / HTTPChaos CR deleted: `kubectl delete -f chaos-experiment.yaml`
- [ ] Verify no chaos CRs remain: `kubectl get podchaos,networkchaos,httpchaos,stresschaos -A`

### Temporary Monitoring Resources
- [ ] Temporary CloudWatch alarms created for this experiment deleted (if not reused)
- [ ] Custom metric dashboards removed (if created only for this experiment)

### Notes
{cleanup_notes}
```

## Summary Report

```markdown
# Chaos Engineering Summary Report

## Overview
- Project: {name}
- Architecture pattern: {pattern}
- Experiment period: {range}
- Total experiments: {N} | Passed: {P} | Failed: {F} | Aborted: {A}

## Resilience Maturity Change
- Before experiments: {score}/5.0 (Assessment evaluation)
- After experiments: {score}/5.0 (Experiment validation)

## Risk Validation Status

| Risk ID | Description | Severity | Result | Validation Status | Improvement Priority |
|---------|------|--------|------|---------|-----------|
| R-XXX | ... | 🔴 | FAILED | Risk confirmed ⚠️ | P0 |
| R-YYY | ... | 🔴 | PASSED | Impact manageable ✅ | P2 (downgraded) |

## Improvement Roadmap
1. ...
```

## HTML Report

The HTML version is generated from Markdown content with additional features:
- Inline CSS (no external dependencies, single file works offline)
- Color coding: PASSED=green, FAILED=red, ABORTED=orange
- Metric comparison visualization (CSS bar chart or embedded SVG)
- Experiment timeline diagram
- Responsive layout
