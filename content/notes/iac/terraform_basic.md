# Terraform Important Blocks & Arguments (Simple Notes)

Terraform is an infrastructure tool created by HashiCorp.

These notes explain key Terraform blocks in an easy and practical way.

------------------------------------------------------------------------

## provider

**Definition:**\
Tells Terraform which cloud or service to connect to.

``` hcl
provider "aws" {
  region = "us-east-1"
}
```

------------------------------------------------------------------------

## resource

**Definition:**\
Creates or manages infrastructure like servers or networks.

``` hcl
resource "aws_instance" "my_server" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

------------------------------------------------------------------------

## data

**Definition:**\
Fetches already existing resources.

``` hcl
data "aws_ami" "latest" {
  most_recent = true
}
```

------------------------------------------------------------------------

## variable

**Definition:**\
Stores input values.

``` hcl
variable "instance_type" {
  default = "t2.micro"
}
```

------------------------------------------------------------------------

## output

**Definition:**\
Displays useful results.

``` hcl
output "server_ip" {
  value = aws_instance.my_server.public_ip
}
```

------------------------------------------------------------------------

## locals

**Definition:**\
Stores reusable values.

``` hcl
locals {
  env = "dev"
}
```

------------------------------------------------------------------------

## module

**Definition:**\
Reusable Terraform code.

``` hcl
module "vpc" {
  source = "./vpc-module"
}
```

------------------------------------------------------------------------

# Advanced Terraform Blocks

## count

**Definition:**\
Creates multiple copies of a resource.

``` hcl
resource "aws_instance" "server" {
  count = 3
  instance_type = "t2.micro"
}
```

------------------------------------------------------------------------

## for_each

**Definition:**\
Creates resources using a list or map.

``` hcl
resource "aws_instance" "servers" {
  for_each = ["dev", "test", "prod"]
  instance_type = "t2.micro"
}
```

------------------------------------------------------------------------

## lifecycle

**Definition:**\
Controls how Terraform handles changes.

``` hcl
resource "aws_instance" "server" {
  lifecycle {
    prevent_destroy = true
  }
}
```

------------------------------------------------------------------------

## depends_on

**Definition:**\
Forces resource order.

``` hcl
resource "aws_instance" "app" {
  depends_on = [aws_vpc.main]
}
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

Happy Terraform Learning!
