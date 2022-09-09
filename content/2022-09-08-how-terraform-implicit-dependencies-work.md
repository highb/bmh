+++
title = "How Terraform Implicit Dependencies Work: A Toy Example"
date = 2022-09-09
+++

Have you ever wondered how [Terraform](https://terraform.io) [Implicit Dependencies](https://learn.hashicorp.com/tutorials/terraform/dependencies) actually work in practice?

In today's blog, I'm going to craft a toy example that demonstrates how I understand that they work using `local_file` resources and thus not requiring any cloud connection or even internet connectivity.

<!-- more -->

## What do you mean by _Implicit Dependencies_?

You might be wondering, "Implicit Dependencies... are those like, the stuff artists do to avoid getting a _Parental Advisory: Explicit Content_ sticker on their albums?" Which, yeah, I guess it could be but I'm talking about Terraform here. In Terraform, every time that you run a `terraform plan` or `terraform apply`, the tool is internally building a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) that models the dependencies between resources in your configuration that you specify using [HCL](https://github.com/hashicorp/hcl). The edges in this graph are the dependency relationships between your resources and they are typically inferred by Terraform without you explicitly specifying them. It is possible to specify these dependencies explicitly using the [depends_on meta-argument](https://www.terraform.io/language/meta-arguments/depends_on) but, as I will discuss in this post, that won't necessarily do what you might expect.

If you aren't required to specify anything about the resource dependencies _explicitly_, then how does Terraform know what the _implicit_ dependencies are? Like any good private investigator, Terraform knows who you're talking to. It uses the references that you are already making every time that you reference the [output values](https://www.terraform.io/language/values/outputs) of a resource, data source, or module somewhere else in your configuration and uses those to piece together how all the pieces fit together.

## Connecting All the Dots

The [example provided by Hashicorp for dependencies](https://learn.hashicorp.com/tutorials/terraform/dependencies) is a good guide, but it a dependency of its own that makes it less accessible for some of us. Specifically, it uses the [AWS provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) which is fine if you're an AWS user but what if you're not a DevOps Wizard flying on Cloud to Nirvana? What if you don't even have internet access? How are you supposed to play around with these TF lanaguage constructs then? That's why I decided to play around with [local_file](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file) resources on my laptop instead. 

To get started, I pulled down [Terraform 1.2.9](https://github.com/hashicorp/terraform/releases/tag/v1.2.9) using [asdf](https://asdf-vm.com/).

```zsh
❯ asdf install terraform 1.2.9
Downloading terraform version 1.2.9 from https://releases.hashicorp.com/terraform/1.2.9/terraform_1.2.9_linux_amd64.zip
...
terraform_1.2.9_linux_amd64.zip: OK
Cleaning terraform previous binaries
Creating terraform bin directory
Extracting terraform archive
Time: 0h:00m:13s 
```

[team/main.tf (GitHub)](https://github.com/highb/bmh-code/blob/311dcbad3b3959ef46113cbb132dd800473dfb1a/terraform/implicit-deps/team/main.tf)
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

[main.tf (GitHub)](https://github.com/highb/bmh-code/blob/311dcbad3b3959ef46113cbb132dd800473dfb1a/terraform/implicit-deps/main.tf)
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
