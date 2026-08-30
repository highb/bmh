+++
title = "namor"
date = "2026-01-28"
description = "Corpus-based English-speaker pronounceable name generator that runs entirely in the browser"
+++

[Namor](https://github.com/highb/namor) is a pronounceable-name generator that
uses **corpus-based phonotactics** rather than random letters. Instead of
producing gibberish like "xqztl," it learns which sound combinations are
plausible by analyzing real name patterns.

How it works:

1. **Syllabification** — breaks training names into onset, nucleus, and coda
2. **Frequency extraction** — counts how often each pattern appears in real names
3. **Weighted sampling** — generates new names by probabilistically combining patterns

Trained on 43 language corpora plus 26 mythological name sets. Ships as a
[Vite SPA](https://highb.github.io/namor/) with a
[standalone HTML build](https://highb.github.io/namor/standalone.html) (~11KB,
zero dependencies) that pre-computes frequency tables at build time. All
computation happens in the browser — no server, no API calls.

[TypeScript](https://typescriptlang.org), [Vite](https://vitejs.dev), corpus data from
[MatthiasWinkelmann/firstname-database](https://github.com/MatthiasWinkelmann/firstname-database).
[MIT](https://opensource.org/licenses/MIT) licensed.

Written with Claude / oh-my-pi running open models.