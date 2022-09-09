+++
title = "How Terraform Implicit Dependencies Work: A Toy Example"
date = 2022-09-09
+++

Have you ever wondered how [Terraform](https://terraform.io) [Implicit Dependencies](https://learn.hashicorp.com/tutorials/terraform/dependencies) actually work in practice?

In today's blog, I'm going to craft a toy example that demonstrates how I understand that they work using `local_files` and thus not requiring any cloud connection or even internet connectivity.

<!-- more -->

## What do you mean Implicit Dependencies?

I know you're wondering, "Is that like... the stuff artists do to avoid getting a _Parental Advisory: Explicit Content_ sticket on their albums?" Which, yeah, I guess it could be but I'm talking about Terraform here. In Terraform, every time that you run a `terraform plan` or `terraform apply`, the tool is internally building a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) that models the dependencies between the resources in your configuration.

```terraform
# team/main.tf
variable "team" {
  type = string
}

variable "content" {
  type = string
}

resource "local_file" "contact_point" {
  content = var.content
  filename = "${var.team}.contact"
}

output "content" {
  value = local_file.contact_point.content
}
```

```terraform
# main.tf
terraform {
  required_providers {
  }
}

locals {
  teams = {
    a = {
      content = "hey, I'm a. aaaaaay!"
    },
    b = {
      content = "great team. we change names a lot."
    },
    c = {
      content = "int main () { return 0; }"
    }
  }
}

module "teams" {
  source = "./team"

  for_each = local.teams

  team = each.key
  content = each.value.content
}

resource "local_file" "team_guide" {
  content = join("\n", [for k, v in module.teams : "${k}: ${v.content}"])
  filename = "team_guide.txt"
}
```
