# AWS Route 53 DNS Firewall & VPC Query-Logging — CloudFormation

Protect any VPC from malicious domains **and** capture every DNS query for observability—all in one drop-in stack.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](#license)

## ✨ What you get

| Resource | Purpose |
|----------|---------|
| **`AWS::Route53Resolver::FirewallRuleGroup`** | Blocks domains you supply (sample list included). |
| **`AWS::Route53Resolver::FirewallRuleGroupAssociation`** | Attaches the rule group to your VPC with a configurable priority. |
| **`AWS::Logs::LogGroup`** | Stores every DNS query sent from the VPC. Retention is parameterised. |
| **`AWS::Route53Resolver::ResolverQueryLoggingConfig`** | Streams all queries to CloudWatch Logs. |
| **`AWS::Logs::ResourcePolicy`** | Grants `route53resolver.amazonaws.com` permission to write to the log group. |

> 🔒  **Least-privilege‡**: Nothing outside Route 53 Resolver can write to the log group.  
> 📝 **Auditable**: Every query—blocked or allowed—hits CloudWatch for real-time dashboards or Athena analysis.

## 🚀 Quick start

```bash
aws cloudformation deploy \
  --stack-name r53-dns-firewall \
  --template-file route53-dns-firewall.yaml \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameter-overrides \
      VpcId=vpc-0123456789abcdef0 \
      AssociationPriority=150 \
      LogRetentionDays=30

Parameter	| Default |	Description
---|---|---
VpcId	| —	| Required – Target VPC to protect & monitor
AssociationPriority	| 150 |	Firewall priority (100-9900, unique per VPC)
LogRetentionDays | 30 | CloudWatch retention in days

## 🛠 Changing the block list

Edit `BlockedDomains` in `route53-dns-firewall.yaml`, or pass a different `FirewallDomainListId` if you manage domain lists elsewhere.
