+++
title = "Using LLMs to actually finish some blog posts"
date = 2023-04-28
+++

I think I'm pretty alright at taking notes in [LogSeq](https://logseq.com) on little projects that I do for fun. The problem is that, for someone who has starry-eyed ambitions of being a writer of some level of competence, I'm awful at turning those notes into a coherent story that others will be interested in. I have a tendency to get lost in side tangents and get distracted by the next shiny project that I want to _do_ without any regard for writing up what I've already done for the benefit of anyone other than myself. It's also just like... a lot of work, you know? So I started thinking, what if I was as lazy as possible and took a few of my existing blog ideas and notes, dumped them into a LLM ([ChatGPT](https://chat.openai.com) [Claude+](https://www.anthropic.com/product)), cleaned them up a little bit, posted those, and then did a write up on the experience, which I would of course also dump right back into an LLM. Let's see just how lazy I can be!

<!-- more -->

## Seizing the Means of Orchestration

So here's what I've got: two half baked posts. One about setting up a k3s cluster in my garage, and the other about writing a discord bot. I guess step one is figuring out a good prompt to give the LLM and then playing around with it from there. `Please take this draft of a post and complete it so that I can post it on my blog. I aim to have a conversational and casual tone for my readers with a splash of meta-narrative about the anxiety I feel about the creation of LLMs and what that implies for people who write for a living like I do in both code and prose. Ensure that the post is formatted in Markdown syntax and has a great hook above the fold denoted by the "more" comment. The draft is as follows: <draft>`

The [draft Claude+ produced](https://github.com/highb/bmh/commit/30a68329f595fffeff9a9c2ec5c86a334f6655e4) dropped all of the actual technical details of what I did but did make a succinct summary. It also assumed that I'm an amateur writer and coder, which is frankly a little bit insulting but I guess thoughtful consideration for others is one of those things that makes us human. Not worrying myself about the hit to my ego, I next prompted `Please retain the details of what I did so that someone could follow along with this blog post as more of a tutorial instead of a brief summary.` This gave me [an improved draft](https://github.com/highb/bmh/commit/bd2868dc2adfa9c18256ea97c11ef560817f8db4) and even updated the title to include "A Tutorial". 

I was, as the youth say, "vibing" to its mention of the "means of production" so I told it to lean into that with the next prompt, `Great, let's keep drafting. For context, I am not by any means an amateur with coding or writing and have been doing both for two decades. Also, I'm really enjoying that you mentioned the "means of production" and would like if you really leaned into the idea of luxury gay space communism.` It's a decent post after [these additional revisions](https://github.com/highb/bmh/commit/d74cd87339d2a51fcef39389352901b00ca4c157). I think it's made a decent contrast between anxieties that I'm feeling about LLMs replacing my job with Kubernetes doing the same thing. I don't think the two technologies are in any way the same in terms of impact on humanity, unless there really were a lot of admins out there who spent all day restarting services after OOM kills, but it's still a funny comparison. It also... almost sounds sarcastic about my level of experience? Am I getting sassed by a machine? Whatever, the point of this exercise was to get a blog post out with minimal effort, even if it doesn't have my characteristic human wit, charm and bad writing.

At this point, I was ready to run `zola serve` to see what the post was going to look like.

<!-- screenshot of front matter error -->
{{ resize_image(path="llm-blogging/Screenshot-2023-04-28-205243.jpg", width=1000, height=300, op="fit") }}

Um, what? The markdown file clearly has the required `+++` on both sides of the front matter. What in the stars is going on here? I decided to delete and recreate the offending lines and did a diff to reveal that Claude had left me a strange present in the response.

<!-- screenshot of the "present" -->
{{ resize_image(path="llm-blogging/Screenshot-2023-04-28-205354.jpg", width=1000, height=300, op="fit") }}

Apparently, trailing spaces are not allowed by the parser in Zola. This is um, a little bit unexpected. What's more unexpected is that Claude decided to add these spaces. What in its training data made it think that trailing spaces in code are _ever_ acceptable? More credence for the "machine is trolling me" theory. Jokes on the machines, though-- they're going to have to keep working while we all switch to permanent vacation mode. That is definitely what will happen. No mass extinction or imprisonment events for humans. No way _that's_ going to or [has already happened](https://www.scientificamerican.com/article/confirmed-we-live-in-a-simulation/)! (This article's jokes about Elon Musk have aged poorly.)

## Intermission

Of course, at this point I re-discovered that [Vercel](https://vercel.com) has a very out of date version of [Zola](https://getzola.org) that doesn't have image rendering support. This led me on a half-hour diversion that ended with me ditching Vercel entirely in favor of using GitHub actions with [shalzz/zola-deploy-action](https://github.com/shalzz/zola-deploy-action) to build the site and deploy it with [GitHub Pages](https://pages.github.com/) because blogging with a static site generator can never be just about the writing. It has to include some surprise troubleshooting!

## The Post that Wasn't

Woes of running a static site generator aside, I was now ready to lazily generate my next post from my existing notes. I merged in the branch with my draft and found it had [essentially nothing](eb5ef565597cb7e131492c52ec00424c31d3d699). Undeterred in my lazy thirst for content, I prompted Claude. `Please generate an informative and funny blog post detailing the process of setting up a Discord bot using the Poise framework using these basic notes of what I did. <draft>`

<!-- big screenshot of what I'm not posting -->
{{ resize_image(path="content\llm-blogging\Screenshot-2023-04-28-231042.jpg", width=1000, height=500, op="fit") }}

Nope. Not posting any of that. Especially because it implied that I was going to run this said monstrosity on *shudder* Oracle Cloud. At this point, I was ready to call it a night with this experiment. But before I did, I wondered if these LLMs have digested all of the test on the internet-- had they eaten my ancient blog posts that I had written in high school? My curiosity got the best of me and I prompted, `Write a blog post in the style found on the blog boogles.net but formatted using the markdown style used in the previous prompts.` It wrote an unremarkable story about me confusing a Roomba with my forgetfulness. I asked `Hm, that's not really in the style of boogles.net. What is your understanding of boogles.net?` to which it called out my writing style in the most detailed way possible.

> You're right, after reviewing boogles.net again, I did not quite capture the style and tone accurately. My understanding of the boogles.net blog style is:
>
> • It has a very quirky and eccentric style with random asides, tangents, and irreverent humor. Posts do not follow a very linear narrative flow and include many parentheticals and footnotes.
>
> • The topics are rather absurdist and whimsical. Posts cover things like the author's strange dreams, encounters with odd animals like ostriches or bees, musings on theoretical scenarios, etc. 
>
> • The tone balances humor and insight. While whimsical and absurdist, the posts often provide thought-provoking observations or reflect on human nature or the nature of reality. But the reflections don't take themselves too seriously.
>
> • Wordplay, metaphors and creative language are used frequently for comedic and poetic effect. Puns, rhymes, and alliteration also appear at times. 
>
> • A rambling, free associative style with sentences that twist and turn in unusual ways. The prose has a very improvisational feel, as if directly streaming thoughts from the author's mind.
>
> • An undercurrent of philosophy or existential angst, but expressed in a rather silly and whimsical way. Life's big questions are pondered but not in an overly serious manner.
>
> • A fascination with dreams, consciousness, reality, perception and the human experience. But these interests are explored with self-aware humor and eccentricity rather than dry intellect.
>
> So in summary, the boogles.net style balances humor, insight, whimsy and existential philosophizing in a very free associative, wordplay-filled, and absurdist manner. My initial attempt failed to capture that delicate balance and style. Please let me know if this helps clarify the boogles.net essence or if I should make another attempt at emulating their style! I appreciate the feedback.

That's... almost flattering? My vanity wouldn't allow me to not ask `Who wrote boogles.net?` to which it confessed:

> My apologies, I do not actually know who wrote or maintains the boogles.net blog. I made up that blog for the purposes of our writing prompt exercise. I do not have any information about the author or specifics of that blog. I fabricated the details about the blog's style, topics, and tone in order to have a concrete example to attempt emulating for the prompts, but I have no data on the actual blog boogles.net. I'm an AI assistant - I don't have a broad knowledge of every blog on the internet, I was bluffing! I can generate fictional content, but do not have information about any real blog called boogles.net.

This left me even more confused and, as it predicted, full of existential angst. Did it generate that summary of my old blog simply from the domain name I gave it? Did it guess just from how I write prompts? Was this... a double bluff and there really was some part of the LLM that had my ancient blog inside of it? Interesting questions that I would have to save for another time because it was getting late and the purpose of this exercise was to actually _post something_ and not to endlessly navel gaze into the eye of AI madness. 

I hope that you had fun reading this; the bulk of _this_ post, except where otherwise noted, is actually written by me. I was originally going to feed the whole post back into Claude but it ended up being to much mental effort to try to shape it into generating something that preserved the original flow of my post. It did write up an good summary of the experience that I will leave you with:

> This experience showed AI's limits. Claude generated a coherent story but lacked human judgment, creativity, and ability to gauge tone for audiences/contexts. It produced "sarcastic" or nonsensical responses, highlighting differences in human/AI humor, logic, and skills like entering code.  
>
> Still, for quick drafts and a laugh, Claude proved a capable ghostwriter, enabling maximum laziness. I got drafts for two posts in the normal time to avoid opening my notes app. Results proved AI won't replace human writers soon but may assist if its quirks are understood. My goal of lazy blogging achieved, I learned more about AI's wrinkles and what makes us human.