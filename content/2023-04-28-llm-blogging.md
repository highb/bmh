+++
title = "Using LLMs to actually finish some blog posts"
date = 2023-04-28
+++

I think I'm pretty alright at taking notes in [LogSeq](https://logseq.com) on little projects that I do for fun. The problem is that, for someone who has starry-eyed ambitions of being a writer of some level of competence, I'm awful at turning those notes into a coherent story that others will be interested in. I have a tendency to get lost in side tangents and get distracted by the next shiny project that I want to _do_ without any regard for writing up what I've already done for the benefit of anyone other than myself. It's also just like... a lot of work, you know? So I started thinking, what if I was as lazy as possible and took a few of my existing blog ideas and notes, dumped them into a LLM ([ChatGPT](https://chat.openai.com) [Claude+](https://poe.com)), cleaned them up a little bit, posted those, and then did a write up on the experience, which I would of course also dump right back into an LLM. Let's see just how lazy I can be!

<!-- more -->

## Seizing the Means of Orchestration

So here's what I've got: two half baked posts. One about setting up a k3s cluster in my garage, and the other about writing a discord bot. I guess step one is figuring out a good prompt to give the LLM and then playing around with it from there. `Please take this draft of a post and complete it so that I can post it on my blog. I aim to have a conversational and casual tone for my readers with a splash of meta-narrative about the anxiety I feel about the creation of LLMs and what that implies for people who write for a living like I do in both code and prose. Ensure that the post is formatted in Markdown syntax and has a great hook above the fold denoted by the "more" comment. The draft is as follows: <draft>`

The [draft Claude+ produced](https://github.com/highb/bmh/commit/30a68329f595fffeff9a9c2ec5c86a334f6655e4) dropped all of the actual technical details of what I did but did make a succinct summary. It also assumed that I'm an amateur writer and coder, which is frankly a little bit insulting but I guess thoughtful consideration for others is one of those things that makes us human. Not worrying myself about the hit to my ego, I next prompted `Please retain the details of what I did so that someone could follow along with this blog post as more of a tutorial instead of a brief summary.` This gave me [an improved draft](https://github.com/highb/bmh/commit/bd2868dc2adfa9c18256ea97c11ef560817f8db4) and even updated the title to include "A Tutorial". 

I was, as the youth say, "vibing" to its mention of the "means of production" so I told it to lean into that with the next prompt, `Great, let's keep drafting. For context, I am not by any means an amateur with coding or writing and have been doing both for two decades. Also, I'm really enjoying that you mentioned the "means of production" and would like if you really leaned into the idea of luxury gay space communism.` It's a decent post after [these additional revisions](https://github.com/highb/bmh/commit/d74cd87339d2a51fcef39389352901b00ca4c157). I think it's made a decent contrast between anxieties that I'm feeling about LLMs replacing my job with Kubernetes doing the same thing. I don't think the two technologies are in any way the same in terms of impact on humanity, unless there really were a lot of admins out there who spent all day restarting services after OOM kills, but it's still a funny comparison. It also... almost sounds sarcasic about my level of experience? Am I getting sassed by a machine? Whatever, the point of this exercise was to get a blog post out with minimal effort, even if it doesn't have my characteristic human wit, charm and bad writing.

At this point, I was ready to run `zola serve` to see what the post was going to look like.

<!-- screenshot of front matter error -->
{{ resize_image(path="content\llm-blogging\Screenshot 2023-04-28 205243.jpg", width=1000, height=100, op="fit") }}

Um, what? The markdown file clearly has the required `+++` on both sides of the front matter. What in the stars is going on here? I decided to delete and recreate the offending lines and did a diff to reveal that Claude had left me a strange present in the response.

<!-- screenshot of the "present" -->
{{ resize_image(path="content\llm-blogging\Screenshot 2023-04-28 205354.jpg", width=1000, height=100, op="fit") }}

Apparently trailing spaces are not allowed by the parser in Zola. This is um, a little bit unexpected. What's more unexpected is that Claude decided to add these spaces. What in it's training data made it think that trailing spaces in code are _ever_ acceptable? More credence for the "machine is trolling me" theory. Jokes on the machines, though-- they're going to have to keep working while we all switch to permanent vacation mode.