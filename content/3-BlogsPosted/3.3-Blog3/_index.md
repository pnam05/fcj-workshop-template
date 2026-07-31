---
title: "Blog 3"
date: 2026-07-10
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# INFRASTRUCTURE MANAGEMENT WITH TERRAFORM — BEYOND CLICKING ON THE CONSOLE

### 1. Introduction

During the journey of learning and building cloud projects, engineers often go through a very familiar path: initially, to set up a system consisting of VPC, EC2, RDS, S3, and other services, we log into the AWS Management Console and click through buttons one by one. This approach is fast and intuitive when getting started.

However, as the project grows, critical questions arise: **How can we replicate this entire infrastructure to Staging or Production environments without making manual errors?** If someone accidentally misconfigures a Security Group on the Console, how do we restore it?

This led the team to adopt **Terraform** and the **Infrastructure as Code (IaC)** mindset — managing cloud infrastructure entirely through source code rather than manual operations.

---

### 2. 5 Core Infrastructure Management Lessons with Terraform

#### 2.1. Infrastructure as Code (IaC) – Turning Infrastructure into Code Files

Instead of remembering manual click sequences, Terraform enables defining all resources (Servers, Networks, Databases, Firewalls) using configuration files written in HCL (HashiCorp Configuration Language).

A simple example to provision an EC2 instance:
```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"

  tags = {
    Name = "WebServer-Dev"
  }
}
```

By defining infrastructure as code, the entire architecture can be stored in Git repositories, track version history, undergo code reviews before deployment, and be easily reused anywhere.

#### 2.2. The "Plan" and "Apply" Workflow – Avoiding Surprises on Production

One of the most valuable features in Terraform is the execution preview mechanism before applying changes to live environments. The standard workflow consists of 3 key steps:

- **terraform init:** Initializes the workspace and downloads necessary providers (AWS, Azure, GCP, etc.).
- **terraform plan:** Compares current code against live infrastructure and displays exact changes: Additions (+), Modifications (~), or Deletions (-).
- **terraform apply:** Executes API calls to AWS to provision specified resources only after explicit confirmation (yes).

The **plan** capability minimizes risks of accidental database deletions or critical misconfigurations — common pitfalls in manual console administration.

#### 2.3. Safely Managing Infrastructure State with terraform.tfstate

In team environments, a primary challenge is: **How does Terraform track which resources have already been created versus those that have not?**

Terraform relies on a state file named **terraform.tfstate** to track infrastructure status. Beginners often make the mistake of keeping **.tfstate** files on local machines or committing them to Git repositories:

- **State Concurrency Conflict:** Simultaneous **apply** execution by multiple team members causes state corruption.
- **Security Leak Risks:** State files store sensitive data (such as DB passwords and tokens), creating severe leak risks if pushed to public repositories.

**Standard Solution:** Adopt a **Remote Backend** (storing state files in Amazon S3) combined with a **DynamoDB Table** for state locking. When an engineer executes changes, DynamoDB locks the state file to prevent concurrent modifications.

#### 2.4. Code Reusability with Terraform Modules

As projects expand, defining all resources in a single **main.tf** file creates monolithic and unmaintainable codebases.

By packaging related resources into reusable Modules (e.g., VPC Module, RDS Module, EKS Module), teams can seamlessly reuse configurations across environments:
```hcl
module "vpc_dev" {
  source     = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
  env        = "dev"
}

module "vpc_prod" {
  source     = "./modules/vpc"
  cidr_block = "10.1.0.0/16"
  env        = "prod"
}
```

This guarantees 100% architectural consistency between Production and Development environments, differing only in resource scale parameters.

#### 2.5. Managing "Drift" – When Infrastructure Changes via Console

In production environments, engineers may occasionally make direct changes on the AWS Console, such as modifying a Security Group or resizing an Instance without updating code. This phenomenon is known as **Infrastructure Drift**.

Running **terraform plan** scans the live environment, detects discrepancies between code and reality, and automatically proposes changes to align infrastructure back to the defined code state. This maintains infrastructure as a **Single Source of Truth**.

---

### 3. Solving Real-World Operational Challenges

#### 3.1. Centralized State Management & State Locking

In collaborative team setups, maintaining state integrity is vital. Utilizing Amazon S3 as a Remote Backend alongside DynamoDB State Locking prevents concurrent write conflicts while securing sensitive data with S3 server-side encryption.

#### 3.2. Multi-Environment Standardization via Modular Architecture

Maintaining parity across Dev, Staging, and Production environments manually is prone to configuration drift. Packaging infrastructure components into standardized Terraform Modules enables rapid environment provisioning and guarantees consistent architecture across tiers.

---

### 4. Conclusion

Cloud infrastructure implementation proves that: **Infrastructure is not just hardware or cloud services — infrastructure is Code.**

Core governance principles to follow:

- Never perform manual modifications on the Console once managed by Terraform
- Protect State files carefully: Always use Remote Backends (S3 + DynamoDB) with state encryption
- Enforce CI/CD pipelines: Integrate **terraform plan** steps into Pull Requests for team review before merging and applying
- Adopt Modular design patterns to ensure maintainability, reusability, and scalability

Transitioning from a "ClickOps" mentality to "Infrastructure as Code" requires an initial investment in learning syntax and tooling. However, as systems grow in scale, IaC becomes an indispensable practice for building standardized, reliable, and production-grade architectures.

---

**Authors:** Thành Nhân, Nguyễn Cảnh Nguyên, Nguyễn Trọng Nhân, Nam Phan, Nguyễn Bá Nam.

**Link Blog:** [Manage Infrastructure with Terraform — Beyond Clicking on the Console](https://www.facebook.com/groups/awsstudygroupfcj/posts/2229938421104451?notif_id=1785478381072886&notif_t=tagged_with_story&ref=notif)

**References:**
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform Recommended Best Practices](https://developer.hashicorp.com/terraform/tutorials)
- [AWS Backend S3 & DynamoDB](https://developer.hashicorp.com/terraform/language/settings/backends/s3)
