## Terraform Blocks & Arguments

Terraform is an infrastructure tool created by HashiCorp.

These notes explain key Terraform blocks in an easy and practical way.

------------------------------------------------------------------------

### 1) terraform Block
**What it is:**\
Main configuration block for Terraform settings like required version, providers, and backend.

**Why used:**\
Controls Terraform behavior and dependencies.

**example:**
```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

```
---

### 2) provider

**What it is:**\
Tells Terraform which cloud/service to talk to and how.

**Why used:**\
Without provider → Terraform doesn’t know where to create resources.

**example:**
``` hcl
  provider "aws" {
  region = "us-east-1"
}
```

------------------------------------------------------------------------

### 3) resource

**What it is:**\
Actual infrastructure you want to create.

**example:**
``` hcl
resource "aws_instance" "my_server" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```
**Quick meaning:**\
- aws_instance → resource type
- my_server → local name

------------------------------------------------------------------------

### 4) variable

**What it is:**\
Input values to make configs reusable.

**example:**
``` hcl
  variable "instance_type" {
  default = "t2.micro"
}
```

**Use it:**
``` hcl
instance_type = var.instance_type
```
------------------------------------------------------------------------

### 5) output

**What it is:**\
Shows important values after terraform apply.

**example:**
``` hcl
  output "server_ip" {
  value = aws_instance.my_server.public_ip
}
```

------------------------------------------------------------------------

### 6) locals

**What it is:**\
Temporary variables inside Terraform for calculations or reuse.

**example:**
``` hcl
locals {
  app_name = "my-app"
}

resource "aws_s3_bucket" "bucket" {
  bucket = "${local.app_name}-bucket"
}

```

------------------------------------------------------------------------

### 7) module

**What it is:**\
Reusable Terraform code (like functions in programming).

**example:**
``` hcl
module "vpc" {
  source = "./modules/vpc"
  cidr   = "10.0.0.0/16"
}
```

------------------------------------------------------------------------

### 8) data

**What it is:**\
Used to fetch existing resources instead of creating new ones.

**example:**
``` hcl
  data "aws_ami" "latest" {
  most_recent = true
}
```

------------------------------------------------------------------------


## Important Resource Arguments

These are commonly used across many resources.

-----

### 1) count

**What it does:**\
Creates multiple copies of a resource.

**example:**
``` hcl
resource "aws_instance" "server" {
  count = 3
  instance_type = "t2.micro"
}
```

**Access:**
``` hcl
aws_instance.web[0].id
```

------------------------------------------------------------------------

### 2) for_each

**What it does:**\
Creates resources based on a map or set.

**example:**
``` hcl
resource "aws_s3_bucket" "bucket" {
  for_each = {
    dev  = "dev-bucket"
    prod = "prod-bucket"
  }

  bucket = each.value
}

```

------------------------------------------------------------------------

### 3) lifecycle

**What it does:**\
Controls how Terraform handles changes.

**Common options:**
- create_before_destroy
- prevent_destroy
- ignore_changes

**example:**
``` hcl
resource "aws_instance" "web" {
  ami           = "ami-123"
  instance_type = "t2.micro"

  lifecycle {
    create_before_destroy = true
  }
}

```

------------------------------------------------------------------------

### 4) depends_on

**What it does:**\
Forces resource creation order.

**example:**
``` hcl
resource "aws_instance" "app" {
  depends_on = [aws_vpc.main]
}
```

------------------------------------------------------------------------

### 5) provisioner (less used now, but still seen)

**What it does:**\
Runs scripts after resource creation.

**example:**
``` hcl
provisioner "local-exec" {
  command = "echo Instance created"
}
```
------------------------------------------------------------------------

## Backend Block (Very Important in Real Projects)

------------------------------------------------------------------------

### Backend

**What it is:**\
Where Terraform state file is stored.

**Example (S3 remote state)**
``` hcl
terraform {
  backend "s3" {
    bucket = "tf-state-bucket"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```

------------------------------------------------------------------------

## Widely Used Modern Concepts (Must Know)

------------------------------------------------------------------------

### 1) Remote State

Store state in:
  - S3
  - Terraform Cloud
  - Azure Storage
  - GCS

**Why:** Team collaboration + locking.

------------------------------------------------------------------------

### 2) Workspaces

Used to manage multiple environments.

``` hcl
terraform workspace new dev
terraform workspace new prod
```
------------------------------------------------------------------------

### 3) Dynamic Blocks

Generate nested blocks programmatically.

``` hcl
dynamic "ingress" {
  for_each = var.ports
  content {
    from_port = ingress.value
    to_port   = ingress.value
    protocol  = "tcp"
  }
}
```
------------------------------------------------------------------------

### 4) Terraform Functions

**Common ones:**
  - lookup()
  - join()
  - split()
  - length()
  - upper()
  - lower()


``` hcl
name = upper("dev-server")
```


------------------------------------------------------------------------

## Quick Memory Trick

provider → connect\
resource → create\
data → read\
count → many copies\
for_each → loop\
lifecycle → control changes\
depends_on → order

------------------------------------------------------------------------

