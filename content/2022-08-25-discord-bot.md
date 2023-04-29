+++
title = "Writing a Discord Bot in Rust with Poise"
date = 2022-08-25
+++

A work in progress

<!-- more -->

## Why?

Because it's fun.

```rust
#[tokio::main]
async fn main() {
    let framework = poise::Framework::builder()
        .options(poise::FrameworkOptions {
            commands: vec![age(), register()],
            ..Default::default()
        })
        .token(std::env::var("DISCORD_TOKEN").expect("missing DISCORD_TOKEN"))
        .intents(serenity::GatewayIntents::non_privileged())
        .user_data_setup(move |_ctx, _ready, _framework| Box::pin(async move { Ok(Data {}) }));

    framework.run().await.unwrap();
}
```

1) made a github repo
2) cargo init
3) Setup GHA
    Reference zero to prod
4) Cross-compile to aarch64 for Pi
    Reference https://www.rohanjain.in/cargo-cross/
    Better reference that uses `cross` https://kerkour.com/rust-cross-compilation




