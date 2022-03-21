---
title: 'Pleroma Setup Write-up'
date: 2021-02-18
tags:
  - social network
  - google cloud
  - pleroma
---

[Pleroma](https://pleroma.social/) is an open source social network platform that is a part of the [Fediverse](https://fediverse.party/en/fediverse). In this write-up, I will be describing my experience with installing the [OTP release](https://erlang.org/doc/design_principles/release_structure.html) on a Linux [VM in Google Cloud](https://cloud.google.com/compute/) by [following the official documentation](https://docs-develop.pleroma.social/backend/installation/otp_en/).

For someone who works with [Kubernetes clusters](https://kubernetes.io/) daily and deploys to them using [Helm](https://helm.sh/), the idea of manually configuring a VM to run my application is an adorable flashback to a simpler time. It brings me back to my days as a student systems administrator at Oregon State or the very early days wherein I would run Unreal Tournament servers from my home Linux computer. Are you feeling the nostalgia?

# Pre-requisites
>    A machine running Linux with GNU (e.g. Debian, Ubuntu) or musl (e.g. Alpine) libc and x86_64, aarch64 or armv7l CPU, you have root access to. If you are not sure if it's compatible see Detecting flavour section below
>    A (sub)domain pointed to the machine

I'm going to handwave over this a bit, Google Cloud and my DNS provider Cloudflare are going to have much better docs on it than I am. Aside from a little annoyance with setting up Google Cloud to play nicely with my Legacy GSuite Account, getting a e2-small (2 vCPUs, 2 GB memory) with a static IP setup and an A Record from CloudFlare was easy enough.

# Installing the required packages

A pretty uneventful set of packages. [The docs describe each one's purpose](https://docs-develop.pleroma.social/backend/installation/otp_en/#installing-the-required-packages). Went ahead and installed them on the VM over SSH.

# [Configuring PostgreSQL](https://docs-develop.pleroma.social/backend/installation/otp_en/#configuring-postgresql)

The developer docs provide some useful tuning config for a small 2GB/2CPU instance like I've provisioned so I'm popping the config values below into `/etc/postgresql/11/main/postgresql.conf`. Beware of just pasting those values in, they already exist in the config! To save myself future confusion, I uncommented/updated the values provided in the config file by the system package. The values check out with what I've seen previously coming out of [PGTune](https://pgtune.leopard.in.ua/#/) in my professional life.

```ini
shared_buffers = 512MB
effective_cache_size = 1536MB
maintenance_work_mem = 128MB
work_mem = 26214kB
max_worker_processes = 2
max_parallel_workers_per_gather = 1
max_parallel_workers = 2
```

# [Installing Pleroma](https://docs-develop.pleroma.social/backend/installation/otp_en/#installing-pleroma)

Next up, making a user for the service, downloading the release, making a bunch of directories, etc. The developer includes what is essentially a shell script in the docs, so I copied the whole thing and put it in a shell script with a few tweaks:
* Took the arch detection code from the top of the doc and injected it as below. This makes the script auto-detect the arch, which afaik is the only input needed on this script.

```bash
arch="$(uname -m)";if [ "$arch" = "x86_64" ];then arch="amd64";elif [ "$arch" = "armv7l" ];then arch="arm";elif [ "$arch" = "aarch64" ];then arch="arm64";else echo "Unsupported arch: $arch">&2;fi;if getconf GNU_LIBC_VERSION>/dev/null;then libc_postfix="";elif [ "$(ldd 2>&1|head -c 9)" = "musl libc" ];then libc_postfix="-musl";elif [ "$(find /lib/libc.musl*|wc -l)" ];then libc_postfix="-musl";else echo "Unsupported libc">&2;fi;echo "$arch$libc_postfix"
export FLAVOUR="${arch}${libc_postfix}"
```

Script ran with no issues. The little auto-configuration wizard that Pleroma includes is pretty easy to breeze through. Once it was up, standard [Let's Encrypt](https://letsencrypt.org/) SSL setup. I'll probably transition away from it and use the CloudFlare provided Origin certs eventually, but this let me turn on Strict SSL faster and with minimal hassle so It Works For Now(tm). Pleroma provided an Nginx config that I only needed to sed the `example.tld` out of and copy over, and then it's off to the races with [a functioning instance](https://tomodachi.club)! Don't forget to setup Let's Encrypt auto-renewal, folks.

# Summary

Overall, the process wasn't too bad, once I quieted the voice in my head that tells me to run everything in Kubernetes and decided I was fine with running directly on a VM instead. Happy microblogging!
