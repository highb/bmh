+++
title = "asdf-teleport-ent"
date = "2022-03-15"
description = "asdf/mise plugin for Teleport Enterprise CLI version management"
+++

[asdf-teleport-ent](https://github.com/highb/asdf-teleport-ent) is an
[asdf](https://asdf-vm.com) and [mise](https://mise.jdx.dev) plugin for
managing [Teleport Enterprise](https://goteleport.com) CLI versions (`tsh`, `tctl`, `teleport`).

Built because I kept jumping between Teleport versions when testing upgrades and
didn't want to manage symlinks by hand. Works with both asdf and mise:

```sh
# asdf
asdf plugin add teleport-ent
asdf install teleport-ent 14.2.2
asdf set teleport-ent 14.2.2

# mise
mise plugin install teleport-ent
mise install teleport-ent@14.2.2
mise global teleport-ent@14.2.2
```

Plain shell — depends on nothing but `bash`, `curl`, and `tar`.
[Apache 2.0](https://opensource.org/licenses/Apache-2.0) licensed.