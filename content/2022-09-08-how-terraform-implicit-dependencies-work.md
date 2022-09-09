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
❯ asdf global terraform 1.2.9

❯ terraform version
Terraform v1.2.9
on linux_amd64
```

Next I wrote up these files: 

[main.tf (GitHub)](https://github.com/highb/bmh-code/blob/1f95dd278f0e1378b8f49a9448f95cebdabe81bc/terraform/implicit-deps/main.tf)
```terraform
locals {
  teams = {
    a = {
      summary = "hey, I'm a. aaaaaay!"
    },
    b = {
      summary = "great team. we change names a lot. bi-weekly, at least."
    },
    c = {
      summary = "int main () { return 0; }"
    }
  }
}

module "teams" {
  source = "./team"

  for_each = local.teams

  team = each.key
  summary = each.value.summary
}

resource "local_file" "team_guide" {
  content = join("\n", [for k, v in module.teams : join(": ", [k, v.content])])
  filename = "team_guide.txt"
}
```

[team/main.tf (GitHub)](https://github.com/highb/bmh-code/blob/1f95dd278f0e1378b8f49a9448f95cebdabe81bc/terraform/implicit-deps/team/main.tf)
```terraform
variable "team" {
  type = string
}

variable "summary" {
  type = string
}

resource "local_file" "team_data" {
  content = var.summary
  filename = "${var.team}.data"
}

output "content" {
  value = local_file.team_data.content
}
```

During the initial `terraform apply` to create the initial state, we can see that it knows that it must create each of the `module.teams` before the `team_guide`. Output is abbreviated.

```zsh
❯ terraform apply
...
  # local_file.team_guide will be created
  + resource "local_file" "team_guide" {
      + content              = <<-EOT
            a: hey, I'm a. aaaaaay!
            b: great team. we change names a lot. bi-weekly, at least.
            c: int main () { return 0; }
        EOT
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "team_guide.txt"
      + id                   = (known after apply)
    }

  # module.teams["a"].local_file.team_data will be created
  + resource "local_file" "team_data" {
      + content              = "hey, I'm a. aaaaaay!"
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "a.data"
      + id                   = (known after apply)
    }

  # module.teams["b"].local_file.team_data will be created
  + resource "local_file" "team_data" {
      + content              = "great team. we change names a lot. bi-weekly, at least."
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "b.data"
      + id                   = (known after apply)
    }

  # module.teams["c"].local_file.team_data will be created
  + resource "local_file" "team_data" {
      + content              = "int main () { return 0; }"
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "c.data"
      + id                   = (known after apply)
    }
...
module.teams["b"].local_file.team_data: Creating...
module.teams["a"].local_file.team_data: Creating...
module.teams["c"].local_file.team_data: Creating...
module.teams["a"].local_file.team_data: Creation complete after 0s [id=fc2e8a361bfa03467011fc4eaffb66e25edf10fa]
module.teams["c"].local_file.team_data: Creation complete after 0s [id=aa42d468cc318c333239fdd23bea3c498d755a56]
module.teams["b"].local_file.team_data: Creation complete after 0s [id=4e6f79afa05a3112ec94f284923bed560435504c]
local_file.team_guide: Creating...
local_file.team_guide: Creation complete after 0s [id=400f628fc381aad58ca6cf47b75468c9535f2f87]

Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

I can then make changes to the `locals` defined here and when I do `terraform apply`, it automatically knows that it needs to destroy the `team_guide` resource _before_ it updates the `teams` modules.

```zsh
❯ terraform apply
...
  # local_file.team_guide must be replaced
-/+ resource "local_file" "team_guide" {
      ~ content              = <<-EOT # forces replacement
            a: hey, I'm a. aaaaaay!
          - b: great team. we change names a lot. bi-weekly, at least.
          + b: great team. we change names a lot. weekly, at least.
            c: int main () { return 0; }
        EOT
      ~ id                   = "400f628fc381aad58ca6cf47b75468c9535f2f87" -> (known after apply)
        # (3 unchanged attributes hidden)
    }

  # module.teams["b"].local_file.team_data must be replaced
-/+ resource "local_file" "team_data" {
      ~ content              = "great team. we change names a lot. bi-weekly, at least." -> "great team. we change names a lot. weekly, at least." # forces replacement
      ~ id                   = "4e6f79afa05a3112ec94f284923bed560435504c" -> (known after apply)
        # (3 unchanged attributes hidden)
    }
...
local_file.team_guide: Destroying... [id=400f628fc381aad58ca6cf47b75468c9535f2f87]
local_file.team_guide: Destruction complete after 0s
module.teams["b"].local_file.team_data: Destroying... [id=4e6f79afa05a3112ec94f284923bed560435504c]
module.teams["b"].local_file.team_data: Destruction complete after 0s
module.teams["b"].local_file.team_data: Creating...
module.teams["b"].local_file.team_data: Creation complete after 0s [id=2c2d10292d625d948193136bfa981ffa84c1a6dd]
local_file.team_guide: Creating...
local_file.team_guide: Creation complete after 0s [id=f5533e02419d3af8acb8736662de1eb54e755544]

Apply complete! Resources: 2 added, 0 changed, 2 destroyed.
```

So, how does this actually work? The answer is here in the content attribute of `team_guide`.

```terraform
resource "local_file" "team_guide" {
  content = join("\n", [for k, v in --> *module.teams* <-- : join(": ", [k, v.content])])
  filename = "team_guide.txt"
}
```

Because this `for each` is referencing `module.teams`, Terraform creates an implicit dependency between these resources. That's it! You could technically add an explicit `depends_on` to the resource but it wouldn't change anything about Terraform's resource graph.

One tricky thing that I learned about the dependency graph when using `depends_on` is that it doesn't work the way that I would have expected it to in a system like Puppet which refreshes it's "state" every time it runs (because, for the most part, it doesn't have any state). To demonstate this, let's modify the configuration in `main.tf` so that the file content is gathered using the `file()` function, instead (ignoring that this would not apply on a fresh state because the files wouldn't exist to be read by `file()`.)

```terraform
resource "local_file" "team_guide" {
  content = join("\n", [for k, v in local.teams : "${k}: ${file(k)}"])
  filename = "team_guide.txt"
}
```

Now if we modify one of the `team` again and `terraform apply`, we see that the only change that is picked up is on the the change to the team itself and not the `team_guide` which is reading in the file.

```terraform
  # module.teams["b"].local_file.team_data must be replaced
-/+ resource "local_file" "team_data" {
      ~ content              = "great team. we change names a lot. weekly, at least." -> "great team. we don't change our name often." # forces replacement
      ~ id                   = "2c2d10292d625d948193136bfa981ffa84c1a6dd" -> (known after apply)
        # (3 unchanged attributes hidden)
    }
...
module.teams["b"].local_file.team_data: Destroying... [id=2c2d10292d625d948193136bfa981ffa84c1a6dd]
module.teams["b"].local_file.team_data: Destruction complete after 0s
module.teams["b"].local_file.team_data: Creating...
module.teams["b"].local_file.team_data: Creation complete after 0s [id=f18ea0e62d069cf8b1383dd84f46ffd3d5d3555d]

Apply complete! Resources: 1 added, 0 changed, 1 destroyed.
```

If we run `terraform apply` _again_ then we will see that the Terraform plan that is implicitly run as part of apply action has picked up the changes to the files on disk, and the diff has been handed that off to apply so now it will converge as expected. For more details on why this is, read about the [resource lifecycle](https://www.terraform.io/internals/lifecycle)

```terraform
  # local_file.team_guide must be replaced
-/+ resource "local_file" "team_guide" {
      ~ content              = <<-EOT # forces replacement
            a: hey, I'm a. aaaaaay!
          - b: great team. we change names a lot. weekly, at least.
          + b: great team. we don't change our name often.
            c: int main () { return 0; }
        EOT
      ~ id                   = "f5533e02419d3af8acb8736662de1eb54e755544" -> (known after apply)
        # (3 unchanged attributes hidden)
    }

Plan: 1 to add, 0 to change, 1 to destroy.
```

If your brain is used to the patterns used by Puppet, you would expect that adding a `depends_on` to the resource would resolve this issue. It does not. The reason for this is that adding this dependency does not change the diff that will be generated during the plan stage. The file being read by `file()` simply will not have changed at that point. There is no concept of a "refresh" between resources in Terraform that would force an update of resources in the dependency graph based on another resource changing. Instead, all of the resources that are known are queried during the plan stage and a diff is generated using those values against the current state file.

I found this to be very counter-intuitive when I first started learning Terraform due to my familiar patterns that I knew from Puppet but once I learned and updated my understanding with how Terraform works, I found that I was able to reason about how my configurations will be applied much better. I hope that this post helped you have some similar epiphanies of your own about Terraform. Thanks for reading!