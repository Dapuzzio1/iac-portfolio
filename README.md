# AWS Infrastructure as Code Portfolio

Production-minded AWS infrastructure built entirely as code, in **AWS CloudFormation** and **Terraform**. Each project is deployable, cost-aware, documented, and built to demonstrate real cloud-engineering practice — not just syntax.

## What this repo demonstrates

- **Two IaC tools, and the trade-offs between them** — the same production VPC built in *both* CloudFormation and Terraform.
- **Networking fundamentals** — multi-AZ VPCs, public/private subnet design, routing, secure boundaries.
- **Highly-available compute** — an internet-facing load balancer fronting a self-healing, auto-scaling EC2 fleet across two Availability Zones.
- **Reusable, team-ready Terraform** — a parameterized VPC **module** that stamps out multiple environments (dev/prod); **remote state with locking** in a versioned S3 bucket; **data sources** (no hardcoded AZs); **`count`** looping; **`locals` + `merge()`** for shared tags; **`.tfvars`** for per-environment values; and **workspaces**.
- **Cost-awareness** — expensive resources (NAT) are optional/conditional; every stack tears down cleanly.
- **Operational discipline** — linted templates (`cfn-lint`), `terraform validate`, least-privilege security groups, no secrets or state in source, reproducible deploys.

## Tech

`AWS` · `CloudFormation` · `Terraform` · `AWS CLI` · `cfn-lint` · `Bash` · `Git`

## Repository structure

```
iac-portfolio/
├── README.md
├── LICENSE
├── .gitignore
├── cloudformation/
│   ├── 00-first-stack/        # fundamentals: the stack lifecycle (one S3 bucket)
│   ├── 01-vpc-foundation/     # production-grade, multi-AZ, cost-aware VPC
│   └── 02-app-tier/           # highly-available web tier (ALB + EC2 Auto Scaling)
└── terraform/
    ├── 01-vpc-foundation/     # VPC in Terraform: variables, data sources, count, locals, tfvars
    └── 02-vpc-module/         # reusable VPC module (dev + prod) + S3 remote state with locking
```

## Projects

| # | Project | Tool | Demonstrates | Status |
|---|---------|------|--------------|--------|
| 00 | First stack (S3 bucket) | CloudFormation | Stack create → update → delete lifecycle | ✅ Complete |
| 01 | VPC network foundation | CloudFormation | Multi-AZ networking, parameters, conditions, cross-stack exports | ✅ Complete |
| 02 | Highly-available web tier | CloudFormation | ALB + EC2 Auto Scaling, cross-stack imports, self-healing across AZs | ✅ Complete |
| 03 | Data, secrets & CI/CD | CloudFormation | RDS, Secrets Manager, change sets, pipelines | ⬜ Planned |
| 04 | VPC foundation (Terraform) | Terraform | Providers, variables, outputs, **data sources**, **count**, **locals**, **.tfvars** | ✅ Complete |
| 05 | Reusable module + remote state | Terraform | Parameterized module, dev/prod, **S3 remote state + locking** | ✅ Complete |
| 06 | Platform & policy-as-code | Terraform | GitHub Actions, tfsec/checkov, Terratest | ⬜ Planned |

## Getting started

**Prerequisites:** an AWS account, the AWS CLI configured with a non-root IAM user, `pip install cfn-lint`, and Terraform (for the `terraform/` projects).

```bash
# CloudFormation — from a project folder
cfn-lint --template vpc.yaml
aws cloudformation deploy --template-file vpc.yaml --stack-name vpc-foundation

# Terraform — from a project folder
terraform init
terraform plan
terraform apply
```

Each project folder has its own README with an architecture diagram, design decisions, and deploy/teardown steps.

## Conventions

- **Commits** follow [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `docs:`, `fix:`, `chore:`).
- **Region:** `us-east-2`. Environments parameterized via `EnvironmentName` (CFN) / `environment` (Terraform).
- **No secrets or state in source** — credentials live only in `~/.aws/`; Terraform state lives in S3; `*.tfstate` and `.terraform/` are gitignored. The `.tfvars` here hold only non-secret values (CIDRs, env names) and are kept to demonstrate multi-environment config.

## License

[MIT](./LICENSE)
