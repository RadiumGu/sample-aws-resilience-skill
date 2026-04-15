# MCP Server Setup Guide

This Skill relies on AWS official MCP Servers to interact with AWS services. Below are the configuration methods for each MCP Server.

> ⚠️ `awslabs.core-mcp-server` is DEPRECATED. Configure standalone MCP Servers directly.
> Migration guide: https://github.com/awslabs/mcp/blob/main/docs/migration-core.md

---

## Required MCP Servers

### 1. aws-api-mcp-server

**Purpose**: FIS experiment create/run/stop, EC2/RDS/EKS resource validation, IAM permission checks

**Installation**: Requires Python 3.10+ and [uv](https://docs.astral.sh/uv/getting-started/installation/)

> ⚠️ **IMPORTANT**: `aws-api-mcp-server` defaults to **read-only mode**.
> Chaos engineering requires write operations (`fis:StartExperiment`,
> `fis:StopExperiment`, `fis:CreateExperimentTemplate`,
> `cloudwatch:PutMetricAlarm`).
>
> You **MUST** set `ALLOW_WRITE_OPERATIONS=true` in the environment variables.
> Without this, the MCP server will silently reject write API calls and may
> disconnect. The Agent will then fall back to AWS CLI, but this wastes time
> and may confuse the workflow.
>
> **Fallback**: If MCP write still fails, use AWS CLI directly:
> `aws fis start-experiment --experiment-template-id <id> --region <region>`

#### Claude Code

```bash
claude mcp add awslabs-aws-api-mcp-server \
  -e AWS_REGION=ap-northeast-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -e ALLOW_WRITE_OPERATIONS=true \
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
        "AWS_REGION": "ap-northeast-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR",
        "ALLOW_WRITE_OPERATIONS": "true"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

#### Cursor / VS Code

Edit `.cursor/mcp.json` or `.vscode/mcp.json`:

```json
{
  "mcpServers": {
    "awslabs.aws-api-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_REGION": "ap-northeast-1",
        "FASTMCP_LOG_LEVEL": "ERROR",
        "ALLOW_WRITE_OPERATIONS": "true"
      }
    }
  }
}
```

---

### 2. cloudwatch-mcp-server

**Purpose**: CloudWatch metric reading, alarm create/query (Stop Conditions), log queries

#### Claude Code

```bash
claude mcp add awslabs-cloudwatch-mcp-server \
  -e AWS_REGION=ap-northeast-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.cloudwatch-mcp-server@latest
```

#### Kiro / Cursor / VS Code

Same format as above, replace package name with `awslabs.cloudwatch-mcp-server@latest`.

---

## Recommended MCP Servers (as needed)

### 3. eks-mcp-server

**Condition**: Configure when the target system uses EKS architecture
**Purpose**: EKS cluster management, K8s resource operations, Pod log viewing

#### Claude Code

```bash
claude mcp add awslabs-eks-mcp-server \
  -e AWS_REGION=ap-northeast-1 \
  -e AWS_PROFILE=default \
  -e FASTMCP_LOG_LEVEL=ERROR \
  -- uvx awslabs.eks-mcp-server@latest
```

---

### 4. chaosmesh-mcp

**Condition**: Configure when EKS cluster has Chaos Mesh installed
**Purpose**: K8s application-layer fault injection (30 tools, covering all CRD types)
**Repository**: https://github.com/RadiumGu/Chaosmesh-MCP

#### Claude Code

```bash
# Clone the repository
git clone https://github.com/RadiumGu/Chaosmesh-MCP.git
cd Chaosmesh-MCP

# Add MCP Server
claude mcp add chaosmesh-mcp \
  -e KUBECONFIG=~/.kube/config \
  -- python3 server.py
```

#### Kiro

```json
{
  "mcpServers": {
    "chaosmesh-mcp": {
      "command": "python3",
      "args": ["/path/to/Chaosmesh-MCP/server.py"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

---

## Complete Configuration Example

Below is the full recommended MCP configuration for this Skill (covering all scenarios):

```json
{
  "mcpServers": {
    "awslabs.aws-api-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_REGION": "ap-northeast-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR",
        "ALLOW_WRITE_OPERATIONS": "true"
      }
    },
    "awslabs.cloudwatch-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cloudwatch-mcp-server@latest"],
      "env": {
        "AWS_REGION": "ap-northeast-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.eks-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.eks-mcp-server@latest"],
      "env": {
        "AWS_REGION": "ap-northeast-1",
        "AWS_PROFILE": "default",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "chaosmesh-mcp": {
      "command": "python3",
      "args": ["/path/to/Chaosmesh-MCP/server.py"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      }
    }
  }
}
```

---

## AWS Credentials Configuration

MCP Servers use the standard AWS credential chain. Recommended methods:

```bash
# Method 1: AWS Profile (recommended)
aws configure --profile default
# Set AWS_PROFILE=default in MCP config

# Method 2: Environment variables
export AWS_ACCESS_KEY_ID=xxx
export AWS_SECRET_ACCESS_KEY=xxx
export AWS_SESSION_TOKEN=xxx  # If using temporary credentials

# Method 3: IAM Role (EC2/EKS environment)
# No extra config needed, auto-uses instance role
```

---

## FIS IAM Role

FIS experiment execution requires a dedicated IAM Role. If one does not exist, Step 4 of the Skill will auto-generate the creation command. Manual creation reference:

```bash
# Create FIS execution role
aws iam create-role \
  --role-name FISExperimentRole \
  --assume-role-policy-document '{
    "Version": "2012-10-17",
    "Statement": [{
      "Effect": "Allow",
      "Principal": {"Service": "fis.amazonaws.com"},
      "Action": "sts:AssumeRole"
    }]
  }'

# Attach policies (select by experiment type)
# EC2 experiments
aws iam attach-role-policy --role-name FISExperimentRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess

# RDS experiments
aws iam attach-role-policy --role-name FISExperimentRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonRDSFullAccess

# EKS experiments
aws iam attach-role-policy --role-name FISExperimentRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy

# Network experiments
aws iam attach-role-policy --role-name FISExperimentRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonVPCFullAccess

# CloudWatch (read alarm state for Stop Conditions)
aws iam attach-role-policy --role-name FISExperimentRole \
  --policy-arn arn:aws:iam::aws:policy/CloudWatchReadOnlyAccess
```

> ⚠️ For production environments, use least-privilege policies instead of FullAccess. The above is for quick-start reference only.

---

## Verify Configuration

After configuration, verify as follows:

```bash
# Verify aws-api-mcp-server
# Run in Claude Code
> /mcp
# Should see awslabs-aws-api-mcp-server status: connected

# Verify AWS credentials
aws sts get-caller-identity

# Verify FIS permissions
aws fis list-experiment-templates

# Verify CloudWatch permissions
aws cloudwatch describe-alarms --max-items 1

# Verify EKS (if configured)
aws eks list-clusters

# Verify Chaos Mesh (if configured)
kubectl get crd | grep chaos-mesh
```

---

## Fallback Without MCP

If MCP Servers are not configured or unavailable, the Skill automatically falls back to direct AWS CLI calls:

| Operation | MCP Method | CLI Fallback |
|------|---------|---------|
| FIS experiments | aws-api-mcp-server | `aws fis create-experiment-template` / `start-experiment` |
| Metric reading | cloudwatch-mcp-server | `aws cloudwatch get-metric-data` |
| Alarm management | cloudwatch-mcp-server | `aws cloudwatch put-metric-alarm` |
| K8s operations | eks-mcp-server | `kubectl` |
| Chaos Mesh | chaosmesh-mcp | `kubectl apply -f` |

Functionality remains complete after fallback, but accuracy is slightly lower (LLM must construct JSON/YAML itself).
