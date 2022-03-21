+++
title = "Migrating to Zola"
date = 2022-03-20
+++

Another year; another static site generator. I'm really enjoying working with [Rust](https://rust-lang.org) so I've migrated this blog to [Zola](https://getzola.com). After explaining why I'm switching, I'll provide a brief walkthough of the steps I had to take.

## Why Zola?

I was previously using [Hexo](https://hexo.io/) for my blog, which is yet another static site generator in JS. It's quite popular, but I don't work very often with the NodeJS ecosystem so I found that every time I wanted to write a new blog post, I had to get aquainted with the latest updates to all the various dependencies -- many of which had security issues. That was all quite a bit of effort for software that is only ever used in the build process to generate static HTML, CSS and JavaScript.

This is all in stark contrast to [Zola](https://getzola.com) which is written in Rust and distributed as a single binary with no dependencies. Sure, if I wanted to build my own binary of Zola I would need to figure out and manage upgrading the dependencies. But as someone who just wants to write a comically simple blog documenting my learning, a single binary with an easily understandable version is just what I need. The site builds in a few milliseconds and has a local hot-reload command to preview my changes. The fact that it was trivial to migrate my existing posts written in Markdown over left me with no reason not to do it. How easy, you ask?

## Generate a new Zola project

I used the default Zola site init tool to make an example set of folders and config, then copied it over my old repo.

```bash
zola init bmh
cp -r bmh/* bmh-github/*
```

## Copy over the relevant config from the old project

Migrating the configuration over from Hexo was mostly a matter of setting a title and enabling some of the features. My blog is pretty simple so YMMV.

## Test it out

Zola has a fast reload command for previewing changes during development. After a little while playing around with that, I had my site setup with a fork of an upstream Zola theme that looks good enough for me.

```bash
zola serve
```

## Setup deployment

I used [Vercel](https://vercel.com/) to do the build and deploy with Hexo. They also support Zola so when I pushed up a new branch with Zola configured instead of Hexo, they automatically generated the site.

## Other goings on

I'm starting out on the [Zero to Production in Rust](https://zero2prod.com) book. I'm on Chapter 3 and it has been a good read so far, although I'm debating if I want to make use of the GitHub actions that are available or if I want to setup some run steps to call the various `cargo` commands myself. For a real "production" experience, it does seem like it would be wise to roll my own instead but I think for the sake of learning it's fine.