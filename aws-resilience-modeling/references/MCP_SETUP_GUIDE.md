# AWS MCP Server Setup Guide

This guide explains how to configure MCP servers for the AWS Resilience Assessment Skill, supporting both **Claude Code** and **Kiro**.

> `awslabs.core-mcp-server` is deprecated (DEPRECATED). Please configure standalone MCP Servers directly.
> Migration guide: https://github.com/awslabs/mcp/blob/main/docs/migration-core.md

---

## Required MCP Servers

### 1. aws-api-mcp-server

**Purpose**: General AWS API access -- Describe/List operations for EC2, RDS, ELB, S3, Lambda, and other resources, covering resource discovery and configuration checks during resilience assessments.

**Installation**: Requires Python 3.10+ and [uv](https://docs.astral.sh/uv/getting-started/installation/)

#### Claude Code

```bash
claude mcp add awslabs-aws-api-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.aws-api-mcp-server@latest
```

#### Kiro

Edit `.kiro/settings/mcp.json`:

```json
{
  "mcpServers": {
    "awslabs.aws-api-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

---

### 2. cloudwatch-mcp-server

**Purpose**: CloudWatch metrics reading, alarm queries, log analysis -- monitoring readiness checks and SLI/SLO analysis during resilience assessments.

#### Claude Code

```bash
claude mcp add awslabs-cloudwatch-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.cloudwatch-mcp-server@latest
```

#### Kiro

Same JSON format as above, replace the package name with `awslabs.cloudwatch-mcp-server@latest`.

---

## Recommended MCP Servers (Configure As Needed)

Based on your AWS architecture, add the following servers as needed:

### 3. eks-mcp-server

**Condition**: Configure when the target system uses EKS architecture
**Purpose**: EKS cluster management, K8s resource operations, Pod log viewing

#### Claude Code

```bash
claude mcp add awslabs-eks-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.eks-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.eks-mcp-server@latest`.

---

### 4. ecs-mcp-server

**Condition**: Configure when the target system uses ECS/Fargate architecture
**Purpose**: ECS cluster, service, and task management

#### Claude Code

```bash
claude mcp add awslabs-ecs-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.ecs-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.ecs-mcp-server@latest`.

---

### 5. dynamodb-mcp-server

**Condition**: Configure when the target system uses DynamoDB
**Purpose**: DynamoDB table operations and queries

#### Claude Code

```bash
claude mcp add awslabs-dynamodb-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.dynamodb-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.dynamodb-mcp-server@latest`.

---

### 6. lambda-tool-mcp-server

**Condition**: Configure when the target system uses Lambda
**Purpose**: Lambda function operations

#### Claude Code

```bash
claude mcp add awslabs-lambda-tool-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.lambda-tool-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.lambda-tool-mcp-server@latest`.

---

### 7. elasticache-mcp-server

**Condition**: Configure when the target system uses ElastiCache
**Purpose**: ElastiCache cluster management

#### Claude Code

```bash
claude mcp add awslabs-elasticache-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.elasticache-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.elasticache-mcp-server@latest`.

---

### 8. iam-mcp-server

**Condition**: Configure when IAM policy and role auditing is needed
**Purpose**: IAM List/Get operations (read-only)

#### Claude Code

```bash
claude mcp add awslabs-iam-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.iam-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.iam-mcp-server@latest`.

---

### 9. cloudtrail-mcp-server

**Condition**: Configure when audit log queries are needed
**Purpose**: CloudTrail event queries

#### Claude Code

```bash
claude mcp add awslabs-cloudtrail-mcp-server \
  -e AWS_REGION=us-east-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.cloudtrail-mcp-server@latest
```

#### Kiro

Same JSON format as the required servers, replace the package name with `awslabs.cloudtrail-mcp-server@latest`.

---

## Complete Configuration Example

The following is the recommended full MCP configuration for resilience assessments (covering common scenarios):

```json
{
  "mcpServers": {
    "awslabs.aws-api-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.cloudwatch-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cloudwatch-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.eks-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.eks-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.ecs-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.ecs-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.dynamodb-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.dynamodb-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.cloudtrail-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cloudtrail-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    }
  }
}
```

> Remove servers you don't need. Minimum configuration requires only `aws-api-mcp-server` + `cloudwatch-mcp-server`.

---

## Read-Only Security Notes

Resilience assessments only require **read-only access**. Read-only characteristics of each MCP Server:

| MCP Server | Read-Only Behavior |
|-----------|-------------------|
| aws-api-mcp-server | Read-only by default (Describe/Get/List operations only) |
| cloudwatch-mcp-server | Read-only by default (Describe/Get/List operations only) |
| cloudtrail-mcp-server | Read-only by default (event queries only) |
| iam-mcp-server | Read-only by default (List/Get operations only) |
| eks-mcp-server | Read-only by default (Describe/List operations only) |
| ecs-mcp-server | Read-only by default (Describe/List operations only) |
| dynamodb-mcp-server | Read-only by default (Describe/List/Query operations only) |

---

## Configure AWS Credentials

```bash
# Method 1: Using AWS CLI configuration
aws configure

# Method 2: Using AWS SSO
aws configure sso

# Verify credentials
aws sts get-caller-identity
```

### Minimum IAM Permission Policy (Read-Only Access)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "rds:Describe*",
        "s3:List*",
        "s3:GetBucket*",
        "lambda:List*",
        "lambda:Get*",
        "dynamodb:List*",
        "dynamodb:Describe*",
        "cloudwatch:Describe*",
        "cloudwatch:Get*",
        "cloudwatch:List*",
        "logs:Describe*",
        "logs:Get*",
        "logs:StartQuery",
        "logs:GetQueryResults",
        "logs:StopQuery",
        "logs:ListLogAnomalyDetectors",
        "logs:ListAnomalies",
        "eks:List*",
        "eks:Describe*",
        "ecs:List*",
        "ecs:Describe*",
        "elbv2:Describe*",
        "apigateway:GET",
        "iam:List*",
        "iam:Get*",
        "cloudtrail:LookupEvents",
        "cloudtrail:GetTrailStatus",
        "ce:GetCostAndUsage",
        "ce:GetCostForecast",
        "pricing:GetProducts",
        "pricing:DescribeServices",
        "elasticache:Describe*",
        "elasticache:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## Verify Configuration

| Tool | Verification Method |
|------|-------------------|
| Kiro | Kiro Feature Panel -> MCP Server View, confirm status is "running" |
| Claude Code | Run `claude mcp list` or type `/mcp` in conversation |

```bash
# Verify AWS credentials
aws sts get-caller-identity

# Verify CloudWatch permissions
aws cloudwatch describe-alarms --max-items 1

# Verify EKS (if configured)
aws eks list-clusters

# Verify ECS (if configured)
aws ecs list-clusters
```

---

## Troubleshooting

### MCP Server Not Connected

```bash
# Verify uv is installed
uv --version

# Manually test MCP server startup (15 second timeout)
timeout 15s uvx awslabs.aws-api-mcp-server@latest 2>&1 || echo "Command completed or timed out"

# Verify AWS credentials
aws sts get-caller-identity
```

### Graceful Degradation Without MCP

If MCP Servers are not configured or unavailable, the Skill automatically falls back to these alternatives (**without directly scanning AWS resources**):
- Analyze IaC code (Terraform/CloudFormation)
- Analyze architecture documentation
- Interactive Q&A

> **Note**: In fallback mode, direct execution of `aws` CLI commands via Bash to scan or operate on AWS resources is **NOT permitted**. All AWS resource access must go through MCP servers.

---

## Advanced Configuration

### Multiple AWS Accounts

Create separate MCP server instances for different accounts:

```json
{
  "mcpServers": {
    "aws-api-production": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "production",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-api-staging": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "staging",
        "AWS_REGION": "us-west-2",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    }
  }
}
```

---

## Migrating from core-mcp-server

If you previously used `awslabs.core-mcp-server`, here is the role-to-standalone-server mapping:

| Former core-mcp-server Role | Replacement Standalone MCP Server |
|-----------------------------|----------------------------------|
| `aws-foundation` | aws-api-mcp-server |
| `monitoring-observability` | cloudwatch-mcp-server, cloudtrail-mcp-server |
| `solutions-architect` | aws-pricing-mcp-server |
| `security-identity` | iam-mcp-server |
| `container-orchestration` | eks-mcp-server, ecs-mcp-server |
| `serverless-architecture` | lambda-tool-mcp-server |
| `nosql-db-specialist` | dynamodb-mcp-server |
| `caching-performance` | elasticache-mcp-server |

Full migration guide: https://github.com/awslabs/mcp/blob/main/docs/migration-core.md

---

## Configuration File Quick Reference

| Item | Claude Code | Kiro |
|------|-------------|------|
| Workspace config path | `.claude/settings.local.json` | `.kiro/settings/mcp.json` |
| User-level config path | `~/.config/claude/settings.json` | `~/.kiro/settings/mcp.json` |
| Check MCP status | `claude mcp list` or `/mcp` | Feature Panel -> MCP Server View |

---

## Reference Resources

- [AWS MCP Servers (Official Repository)](https://github.com/awslabs/mcp)
- [core-mcp-server Migration Guide](https://github.com/awslabs/mcp/blob/main/docs/migration-core.md)
- [CloudWatch MCP Server](https://github.com/awslabs/mcp/tree/main/src/cloudwatch-mcp-server)
- [AWS API MCP Server](https://github.com/awslabs/mcp/tree/main/src/aws-api-mcp-server)
- [EKS MCP Server](https://github.com/awslabs/mcp/tree/main/src/eks-mcp-server)
- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [AWS CLI Configuration Documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
- [uv Installation Guide](https://docs.astral.sh/uv/getting-started/installation/)

---

**Updated: 2026-03-24**
