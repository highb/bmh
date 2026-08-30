+++
title = "wordle-goggles"
date = "2022-01-27"
description = "Wordle Analyzer/Solver that uses NLTK to eliminate uncommon words"
+++

[Wordle Goggles](https://github.com/highb/wordle-goggles) is a Wordle analyzer
and solver that uses **NLTK corpus data** to filter out uncommon words from
candidate lists. When Wordle was at peak hype in early 2022, this tool provided
data-driven guesses instead of relying on human word-frequency intuition.

Uses the Reuters corpus via NLTK to score words by real-world usage frequency,
narrowing the solution space faster than random guessing. Written as a
single-file Python script — run it, feed in your Wordle results, and get the
next best guess.

[Python](https://python.org), [NLTK](https://nltk.org).
[MIT](https://opensource.org/licenses/MIT) licensed.