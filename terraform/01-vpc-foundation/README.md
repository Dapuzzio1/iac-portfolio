# Terraform — VPC Network Foundation (flat)

The VPC network foundation in **Terraform (HCL)** — VPC, internet gateway, two public + two private subnets across two AZs, and public routing — built with variables, data sources, looping, locals, and per-environment value files.

## Architecture

![Architecture](./architecture.svg)

## What it demonstrates

- Terraform **providers** + `required_providers`
- **Resources** and references (`aws_vpc.main.id`), tags
- **Input variables** (`variables.tf`) with types and defaults
- **Data sources** — `aws_availability_zones` looks up the region's AZs live (no hardcoding; portable to any region)
- **Looping with `count`** — one subnet block builds all subnets (`count = length(var.public_subnet_cidrs)`, `count.index`); outputs use the `[*]` splat
- **`locals` + `merge()`** — a shared `common_tags` bundle applied to every resource
- **`.tfvars` files** — per-environment values (`dev.tfvars`, `prod.tfvars`) selected at run time with `-var-file`
- **Outputs**
- Full workflow: `init -> plan -> apply -> destroy`; **local state** (see `../02-vpc-module` for S3 remote state)

## Usage

```bash
terraform init
terraform plan -var-file=dev.tfvars      # or prod.tfvars
terraform apply -var-file=prod.tfvars    # builds prod (10.1.0.0/16)
terraform destroy -var-file=prod.tfvars
```

## Notes

- Region, CIDRs, and environment name are variables — change them in one place, or swap a `.tfvars` file to switch environments without touching code.
- AZs come from a data source (not hardcoded); subnets are generated with `count` from the CIDR lists — add a CIDR and you get another subnet automatically.
- `common_tags` are defined once in `locals` and merged onto every resource.
