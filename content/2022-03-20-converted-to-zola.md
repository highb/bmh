+++
title = "Migrating to Zola"
+++

Another year; another static site generator. I'm really enjoying working with Rust so I've migrated this blog to [Zola](https://getzola.com). I'll provide a brief walkthough of the steps I had to take.

1. Generate a new Zola project

```bash
zola init bmh
cp -r bmh/* bmh-old/*
```

1. Copy over the relevant config from the old project

I was previously using [Hexa]() for my blog.

1. Test it out

```bash
zola serve
```

1. Setup deployment
