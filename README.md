# AWS Infrastructure as Code Portfolio

Production-minded AWS infrastructure built entirely as code, using **AWS CloudFormation** and **Terraform**. Each project is deployable, cost-aware, documented, and built to demonstrate real cloud-engineering practice — not just syntax.

## What this repo demonstrates

- **Two IaC tools, and the trade-offs between them** — the same infrastructure expressed in CloudFormation and (later) Terraform, with commentary on when to reach for each.
- **Networking fundamentals** — VPCs, multi-AZ public/private subnet design, routing, and secure network boundaries.
- **Cost-awareness** — expensive resources (NAT gateways) are parameterized and optional; every stack is designed to be torn down cleanly.
- **Operational discipline** — linted templates (`cfn-lint`), least-privilege IAM, no secrets in source control, reproducible deploys.

## Tech

`AWS` · `CloudFormation` · `Terraform` · `AWS CLI` · `cfn-lint` · `Bash` · `Git`

## Repository structure

```
iac-portfolio/
├── README.md                     # you are here
├── LICENSE
├── .gitignore
├── cloudformation/
│   ├── 00-first-stack/           # fundamentals: the stack lifecycle (one S3 bucket)
│   └── 01-vpc-foundation/        # production-grade, multi-AZ, cost-aware VPC
│       ├── vpc.yaml
│       ├── architecture.svg
│       └── README.md
└── terraform/                    # (coming) the same platform, rebuilt in Terraform
```

## Projects

| # | Project | Tool | Demonstrates | Status |
|---|---------|------|--------------|--------|
| 00 | First stack (S3 bucket) | CloudFormation | Stack create → update → delete lifecycle | ✅ Complete |
| 01 | VPC network foundation | CloudFormation | Multi-AZ networking, parameters, conditions, cross-stack exports | ✅ Complete |
| 02 | Compute + app tier | CloudFormation | ALB + ECS/EC2, IAM, nested stacks | ⬜ Planned |
| 03 | Data, secrets & CI/CD | CloudFormation | RDS, Secrets Manager, change sets, pipelines | ⬜ Planned |
| 04 | VPC foundation (Terraform) | Terraform | Providers, state, remote backend | ⬜ Planned |
| 05 | Modules & multi-env | Terraform | Reusable modules, dev/stage/prod | ⬜ Planned |
| 06 | Platform & policy-as-code | Terraform | GitHub Actions, tfsec/checkov, Terratest | ⬜ Planned |

## Getting started

**Prerequisites:** an AWS account, the AWS CLI configured with a non-root IAM user, and `pip install cfn-lint` for validation.

```bash
# from a project folder, e.g. cloudformation/01-vpc-foundation
cfn-lint --template vpc.yaml
aws cloudformation deploy --template-file vpc.yaml --stack-name vpc-foundation
```

Each project folder has its own README with an architecture diagram, design decisions, and deploy/teardown steps.

## Conventions

- **Commits** follow [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `docs:`, `fix:`, `chore:`).
- **Region:** `us-east-2`. **Environments** are parameterized via `EnvironmentName`.
- **No secrets in source** — credentials live only in `~/.aws/`, never in the repo.

## License

[MIT](./LICENSE)
