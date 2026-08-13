# AWS Infrastructure as Code Portfolio

Production-minded AWS infrastructure built entirely as code, in **AWS CloudFormation** and **Terraform**. Each project is deployable, cost-aware, documented, and built to demonstrate real cloud-engineering practice — not just syntax.

## What this repo demonstrates

- **Two IaC tools, and the trade-offs between them** — the same production VPC built in *both* CloudFormation and Terraform.
- **Networking fundamentals** — multi-AZ VPCs, public/private subnet design, routing, secure boundaries.
- **Highly-available compute** — an internet-facing load balancer fronting a self-healing, auto-scaling EC2 fleet across two Availability Zones.
- **Cost-awareness** — expensive resources (NAT) are optional/conditional; every stack tears down cleanly.
- **Operational discipline** — linted templates (`cfn-lint`), `terraform validate`, least-privilege security groups, no secrets in source, reproducible deploys.

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
    └── 01-vpc-foundation/     # the VPC network foundation, rebuilt in Terraform (HCL)
```

## Projects

| # | Project | Tool | Demonstrates | Status |
|---|---------|------|--------------|--------|
| 00 | First stack (S3 bucket) | CloudFormation | Stack create → update → delete lifecycle | ✅ Complete |
| 01 | VPC network foundation | CloudFormation | Multi-AZ networking, parameters, conditions, cross-stack exports | ✅ Complete |
| 02 | Highly-available web tier | CloudFormation | ALB + EC2 Auto Scaling, cross-stack imports, self-healing across AZs | ✅ Complete |
| 03 | Data, secrets & CI/CD | CloudFormation | RDS, Secrets Manager, change sets, pipelines | ⬜ Planned |
| 04 | VPC foundation (Terraform) | Terraform | Providers, state, HCL, full init/plan/apply/destroy loop | ✅ Complete |
| 05 | Modules & multi-env | Terraform | Reusable modules, remote state, dev/stage/prod | ⬜ Planned |
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
- **Region:** `us-east-2`. Environments parameterized via `EnvironmentName`.
- **No secrets in source** — credentials live only in `~/.aws/`; Terraform state and `.terraform/` are gitignored.

## License

[MIT](./LICENSE)
