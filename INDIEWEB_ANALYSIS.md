# IndieWeb Compatibility Analysis

## Current Status: No IndieWeb Support

### Terminal Theme (hugo-theme-terminal)
The terminal theme by Panr does **not** include microformats2 markup. No `h-entry`,
`h-card`, `p-name`, `dt-published`, or `e-content` classes exist in its templates.

### Custom Layout Overrides
Only one override exists: `layouts/partials/extended_head.html` (GoatCounter analytics).
No microformats2 markup present.

### Webmention / IndieAuth / rel="me"
- No webmention endpoints configured
- No IndieAuth endpoints configured
- No Micropub endpoints configured
- `rel="me"` links exist but are commented out (legacy Zola config in config.toml)

## What Needs to Be Added

### 1. Microformats2 via Layout Overrides
- Override `layouts/_default/single.html` or `layouts/posts/single.html` to wrap posts
  in `h-entry` with `p-name`, `e-content`, `dt-published`, `p-author`
- Add `h-card` markup (homepage or partial)
- Add `h-feed` to list pages

### 2. Endpoint Links (in `layouts/partials/extended_head.html`)
```html
<link rel="webmention" href="https://webmention.io/bmh.io/webmention" />
<link rel="pingback" href="https://webmention.io/bmh.io/xmlrpc" />
<link rel="authorization_endpoint" href="https://indieauth.com/auth" />
<link rel="token_endpoint" href="https://tokens.indieauth.com/token" />
```

### 3. Re-enable rel="me" Links
Activate the commented-out links in config.toml, particularly the Mastodon link
(`https://pdx.sh/@bh`) for IndieAuth identity verification.
