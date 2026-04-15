# FIS Actions Reference (by Service)

## FIS General — API Fault Injection (ec2 and kinesis ONLY)

> ⚠️ These three actions ONLY support `service=ec2` and `service=kinesis`.
> For other AWS services, use their dedicated FIS Actions:
>
> | Service | Dedicated FIS Action |
> |---------|---------------------|
> | DynamoDB | `aws:dynamodb:global-table-pause-replication` |
> | Lambda | `aws:lambda:invocation-add-delay`, `aws:lambda:invocation-error` |
> | EBS | `aws:ebs:pause-volume-io`, `aws:ebs:volume-io-latency` |
> | S3 | `aws:s3:bucket-pause-replication` |
> | RDS | `aws:rds:failover-db-cluster`, `aws:rds:reboot-db-instances` |

| Action | Description |
|--------|------|
| `aws:fis:inject-api-internal-error` | Inject 500 errors into API requests for the target IAM Role |
| `aws:fis:inject-api-throttle-error` | Inject throttling errors |
| `aws:fis:inject-api-unavailable-error` | Inject service unavailable errors |
| `aws:fis:wait` | Wait for a specified duration (for orchestration) |

## EC2
| Action | Description |
|--------|------|
| `aws:ec2:terminate-instances` | Terminate instances (SPOF validation) |
| `aws:ec2:stop-instances` | Stop instances (recoverable) |
| `aws:ec2:reboot-instances` | Reboot instances |
| `aws:ec2:send-spot-instance-interruptions` | Simulate Spot interruptions |
| `aws:ec2:asg-insufficient-instance-capacity-error` | ASG insufficient capacity |
| `aws:ec2:disrupt-network-connectivity` | EC2 network disruption (NACL) |

## EBS
| Action | Description |
|--------|------|
| `aws:ebs:pause-volume-io` | Pause EBS volume IO |

## EKS
| Action | Description |
|--------|------|
| `aws:eks:terminate-nodegroup-instances` | Terminate node group instances (**recommended for node-level faults**) |
| `aws:eks:pod-*` series | Pod fault injection — ⚠️ **Not recommended**: slow initialization (>2min), requires additional SA/RBAC config. Prefer Chaos Mesh for Pod-level faults |

## ECS
| Action | Description |
|--------|------|
| `aws:ecs:drain-container-instances` | Drain container instances |
| `aws:ecs:stop-task` | Stop ECS tasks |
| `aws:ecs:task` series | ECS task-level fault injection |

## RDS
| Action | Description |
|--------|------|
| `aws:rds:failover-db-cluster` | Aurora/RDS cluster failover |
| `aws:rds:reboot-db-instances` | Reboot RDS instances |

## DynamoDB
| Action | Description |
|--------|------|
| `aws:dynamodb:global-table-pause-replication` | Pause global table replication |

## ElastiCache / MemoryDB
| Action | Description |
|--------|------|
| `aws:elasticache:interrupt-cluster-az-power` | ElastiCache AZ power interruption |
| `aws:memorydb:interrupt-cluster-az-power` | MemoryDB AZ power interruption |

## Lambda

> ⚠️ **Prerequisites**: All Lambda FIS actions require the target Lambda function to have:
> 1. **FIS Extension Layer** installed (region-specific ARN from [AWS docs](https://docs.aws.amazon.com/fis/latest/userguide/fis-actions-reference.html#lambda-actions))
> 2. **`AWS_FIS_CONFIGURATION_LOCATION`** environment variable set to the experiment ARN
>
> Without these, FIS actions will silently fail. See `fault-catalog.yaml` for setup commands.
>
> ⚠️ **Common mistake**: Setting `AWS_FIS_CONFIGURATION_LOCATION` to an S3 path
> (e.g., `s3://bucket/config`) will cause FIS to reject the experiment at start with
> _"invalid value for the required environment variable"_. Use the experiment ARN format:
> `arn:aws:fis:<region>:<account>:experiment/<experiment-id>`

| Action | Description |
|--------|------|
| `aws:lambda:invocation-add-delay` | Inject delay into Lambda invocations |
| `aws:lambda:invocation-error` | Inject errors into Lambda invocations |
| `aws:lambda:invocation-http-integration-response` | HTTP integration response injection |

## S3
| Action | Description |
|--------|------|
| `aws:s3:bucket-pause-replication` | Pause cross-region replication |

## Kinesis
| Action | Description |
|--------|------|
| `aws:kinesis:add-put-record-throttle` | Write throttling |

## Network
| Action | Description |
|--------|------|
| `aws:network:disrupt-connectivity` | Subnet/SG-level network disruption (AZ isolation) |
| `aws:network:route-table-disrupt-cross-region-connectivity` | Cross-region route disruption |
| `aws:network:transit-gateway-disrupt-cross-region-connectivity` | TGW cross-region disruption |

## Others
| Action | Description |
|--------|------|
| `aws:arc:start-zonal-autoshift` | Trigger AZ automatic traffic shift |
| `aws:cloudwatch:assert-alarm-state` | Assert alarm state (orchestration condition check) |
| `aws:ssm:send-command` | Execute SSM document on instances |
| `aws:ssm:start-automation-execution` | Run SSM Automation |
| `aws:directconnect:disrupt-connectivity` | Direct Connect disruption |

> Full list: [AWS FIS Actions Reference](https://docs.aws.amazon.com/fis/latest/userguide/fis-actions-reference.html)
