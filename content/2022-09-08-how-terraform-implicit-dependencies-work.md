+++
title = "How Terraform Implicit Dependencies Work: A Toy Example"
date = 2022-09-09
+++

Have you ever wondered how [Terraform](https://terraform.io) [Implicit Dependencies](https://learn.hashicorp.com/tutorials/terraform/dependencies) actually work in practice?

In today's blog, I'm going to craft a toy example that demonstrates how I understand that they work using `local_file` resources and thus not requiring any cloud connection or even internet connectivity.

<!-- more -->

## What do you mean Implicit Dependencies?

TODO: Album cover gif goes here

You might be wondering, "Is that like, the stuff artists do to avoid getting a _Parental Advisory: Explicit Content_ sticker on their albums?" Which, yeah, I guess it could be but I'm talking about Terraform here. In Terraform, every time that you run a `terraform plan` or `terraform apply`, the tool is internally building a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) that models the dependencies between resources in your configuration that you specify using [HCL](https://github.com/hashicorp/hcl). The edges in this graph are the dependency relationships between your resources and they are typically inferred by Terraform without you _explicitly_ specifying them. It is possible to specify these dependencies _explicitly_ using the [depends_on meta-argument](https://www.terraform.io/language/meta-arguments/depends_on) but, as I will discuss in this post, that won't necessarily do what you might expect.

[team/main.tf](https://github.com/highb/bmh-code/blob/311dcbad3b3959ef46113cbb132dd800473dfb1a/terraform/implicit-deps/team/main.tf)
```terraform
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

[main.tf](https://github.com/highb/bmh-code/blob/311dcbad3b3959ef46113cbb132dd800473dfb1a/terraform/implicit-deps/main.tf)
```terraform
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
