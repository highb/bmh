---
title: 'Social Network Roundup'
date: 2020-04-01
tags:
  - social network
  - diaspora
  - mastodon
  - friendica
  - pixelfed
  - kubernetes
  - google cloud
---

Despite [working for Facebook](https://www.linkedin.com/in/brandonhighthesoftwareguy) a few years ago, I'm not a huge fan of the platform. I think that attempting to cram every human being on the planet into one massive social network without any concept of community enforcement is counterproductive and antithetical to how humans are capable of socializing. We evolved to interact with small bands of hunter-gatherers, not thousands of individuals via highly abstract proxies for interaction such as [Likes](https://facebook.com) and [Hearts](https://twitter.com). This has been written about in much more detail than I can do justice by many people much smarter than me so I won't attempt my own ["hot take"](https://en.wikipedia.org/wiki/Hot_take) on it. If you are curious about the concept, look into [Dunbar's Number](https://en.wikipedia.org/wiki/Dunbar%27s_number).

<!-- more -->

Given that preface, I hope that you find it easier to understand why I'm interested in finding alternative social networks. I have no distaste for _idea_ of a social network. To the contrary, I find the general concept of individuals being able to easily share information with each other as a group is often beneficial for everyone involved and has the potential to strengthen our collective powers. Social networking has allowed many people to band together and organize political and activist projects that have done an incredible amount of good in the world. Our feeds have allowed many marginalized and ignored groups to have a voice where the television and newspapers reporters of the past wouldn't have seen a story.

Does a network with that sort of power exist today without being beholden to a mega-corporation? What sort of founding principles would guide a network in a way that it could resist that sort of control and influence while still providing power to the people? I want to find out, so I'm going to attempt setting up as many social networks as I can find and then writing them up here. Before I start out on that quest, I would like to outline the criteria that I'll be looking for:

* Open source: The code for the network must be available freely to everyone under a licence that will not allow it to be hijacked by a massive corporation. This ensures that anyone can run the network and that no one will be allowed to become a gatekeeper. Provided that there are well documented APIs available, I may consider it acceptable if there are closed-source mobile clients as long as other clients are permitted by the licencing.
* Federated: There must be no central control to the network. Each node should be able to freely associate with any other, or choose to completely dissociate with another node. This reflects the nature of human society and allows people to form webs digital webs of trust.
* Community-focused: Nodes in the social network must have good community management features included that allow operators of a node to moderate users and build effective communities where people are not afraid to express themselves.
* User friendly: User interfaces are a critical part of any digital experience and a social network is no different. People have to be able to use the thing in an easy to understand way, even if that means sacrificing [VIM keybindings](https://www.vim.org/).

I'm interested in looking into [Diaspora](https://diasporafoundation.org/), [Mastodon](https://mastodon.social), [Friendica](https://friendi.ca/), and [PixelFed](https://pixelfed.org/). I'll prefer running them in [Kubernetes](https://kubernetes.io/) when at all possible and will consider making upstream contributions to make that easier for others. As I experiment with these platforms, I'll either continue to update this post or make new follow up posts. Thank you for reading!