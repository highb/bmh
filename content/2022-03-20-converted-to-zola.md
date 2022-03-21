+++
title = "Migrating to Zola"
date = 2022-03-20
+++

Another year; another static site generator. I'm really enjoying working with Rust so I've migrated this blog to [Zola](https://getzola.com). I'll provide a brief walkthough of the steps I had to take.

## Generate a new Zola project

I used the default Zola site init tool to make an example set of folders and config, then copied it over my old repo.

```bash
zola init bmh
cp -r bmh/* bmh-github/*
```

## Copy over the relevant config from the old project

I was previously using [Hexo](https://hexo.io/) for my blog, which is yet another static site generator in JS. Migrating the configuration over was mostly a matter of setting a title and enabling some of the features. My blog is pretty simple so YMMV.

## Test it out

Zola has a fast reload command for previewing changes during development. After a little while playing around with that, I had my site setup with a fork of an upstream Zola theme that looks good enough for me.

```bash
zola serve
```

## Setup deployment

I used [Vercel](https://vercel.com/) to do the build and deploy with Hexo. They also support Zola so when I pushed up a new branch with Zola configured instead of Hexo, they automatically generated the site.

## Other goings on

I'm starting out on the [Zero to Production in Rust](https://zero2prod.com) book. I'm on Chapter 3 and it has been a good read so far, although I'm debating if I want to make use of the GitHub actions that are available or if I want to setup some run steps to call the various `cargo` commands myself. For a real "production" experience, it does seem like it would be wise to roll my own instead but I think for the sake of learning it's fine.