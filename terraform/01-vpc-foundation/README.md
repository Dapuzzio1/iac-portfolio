# Terraform — VPC Network Foundation (flat)

The Project 1 VPC, rebuilt in **Terraform (HCL)**. Same production network — VPC, internet gateway, two public + two private subnets across two AZs, and public routing — expressed as Terraform resources with variables, data sources, looping, and outputs.

## Architecture

![Architecture](./architecture.svg)

## What it demonstrates

- Terraform **providers** + `required_providers`
- **Resources** and references (`aws_vpc.main.id`), tags
- **Input variables** (`variables.tf`) with types and defaults
- **Data sources** — `aws_availability_zones` looks up the region's AZs live, so nothing is hardcoded (portable to any region)
- **Looping with `count`** — one subnet block builds all subnets (`count = length(var.public_subnet_cidrs)`, `count.index`), and outputs use the `[*]` splat
- **Outputs**
- The full workflow: `init -> plan -> apply -> destroy`
- **Local state** (see `../02-vpc-module` for the S3 remote-state version)

## Usage

```bash
terraform init
terraform plan
terraform apply     # type yes
terraform destroy   # type yes, when done
```

## Notes

- Region, CIDRs, and environment name are variables — change them in one place.
- AZs come from a data source (not hardcoded), and subnets are generated with `count` from the CIDR lists — add a CIDR to the list and you get another subnet automatically.
- This is the "flat" version (single config). The `02-vpc-module` project refactors it into a **reusable module** with **S3 remote state**.
