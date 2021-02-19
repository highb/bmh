---
title: 'Pleroma Setup Write-up'
date: 2021-02-18 00:00:00
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

A pretty uneventful set of packages. [The docs describe each one's purpose](https://docs-develop.pleroma.social/backend/installation/otp_en/#installing-the-required-packages).
