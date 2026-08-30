+++
title = "pathspec-ruby"
date = "2014-07-10"
description = "Ruby gem for matching .gitignore-style path specifications"
+++

[pathspec-ruby](https://github.com/highb/pathspec-ruby) is a [Ruby](https://ruby-lang.org) gem that
matches `.gitignore`-style path specifications. It follows the full
[gitignore syntax](https://git-scm.com/docs/gitignore) — negation patterns,
directory-only rules, glob wildcards, and precedence ordering.

Ported from the Python [pathspec](https://pypi.org/project/pathspec) library, it
ships a CLI (`pathspec-rb`) and a Ruby API:

```ruby
gitignore = PathSpec.from_filename('.gitignore')
gitignore.match 'coverage/index.html'   # => true
gitignore.match 'important.txt'         # => false (negated)
gitignore.match_tree '/src/repo'        # returns matching file paths
```

Published as the `pathspec` gem on [RubyGems](https://rubygems.org/gems/pathspec). Supports Ruby 3.2 through 4.0.
[Apache 2.0](https://opensource.org/licenses/Apache-2.0) licensed.