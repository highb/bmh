---
title: "Exporting from Proton Pass to 1password"
date: 2025-05-20T11:22:41-07:00
draft: true
---

What a mess. I changed email providers from Fastmail to Proton Mail a while ago. While doing so, I found that they also offered a password manager called Proton Pass. Thinking that being completely on the Proton suite would make my life easier, I moved my personal credentials into Proton Pass.

In time, I would come to regret that decision.

<!--more-->

Before I get to my regrets, I'll first say what is great about Proton Pass. My favorite feature by far is their integration with SimpleLogin, which generates email aliases quickly and easily when I'm signing up for a new account somewhere. Using an alias like this provides protection from spam (marketing or otherwise), as well as potential data breaches, since I can easily redirect the alias into a trash folder that automatically clears or delete the alias entirely, if necessary. The other features it provides—like 2FA/passkey support, dark web monitoring, and group password sharing—are all features of every serious password manager. So while they are good features, they aren't unique.

The tooling around Proton Pass is where things start to get weak. The biggest pain point for me is that it doesn't support storing SSH keys or acting as an SSH agent, so I had to keep 1Password around anyway in order to use my SSH keys. In a similar vein, the op CLI for 1Password is incredibly useful for automation in scripts, but Proton Pass has no CLI and no apparent plans to make one. The browser extensions work alright, but they aren't nearly as good as 1Password's.

Those are all small issues though, and most likely over time, Proton is going to fill that gap. Ultimately, the biggest hurdle was a social one, not a technological one, as is often the case. My wife uses 1Password on a family account with me, and she really wasn't feeling the need to migrate to a completely different tool when 1Password works fine for her. Which, in retrospect for this case, was an extremely wise position to take. Proton Pass isn't THAT much better, so I really should have stayed put and spent my energy exploring a tool that actually provides new capabilities for us, rather than wasting time exploring something with no appreciable benefit.

Once I realized I would be running two password managers side-by-side in this situation I had put myself in, I quickly saw that it was untenable. The two password managers compete for screen real estate in the browser and frequently steal focus from each other, making both tools much worse to use. With this realization, I chose to move back to 1Password—but I ran into a problem: 1Password doesn't support importing from Proton Pass except via ZIP file or CSV, which is incompatible with 1Password's importer and loses 2FA/passkeys and attachments in the process. Oof.

To work around this, I used the following Rosetta Stone-style conversion:

Export a ZIP from Proton Pass web UI via Settings (Gear) > Export > File format (ZIP).
Create a Bitwarden free account. Use the "Import Data" wizard to pull in the ZIP.
Export from Bitwarden web UI via Tools > Export Vault. Choose ".json (Encrypted)" as the file format and set it as "Password protected."
Import into 1Password web UI via the account link in the upper right corner > Import data. Select Bitwarden, pick a new/existing vault, enter the passphrase, and upload the JSON file.
Delete the database exports, wherever they were saved.
After performing these steps, I had all of my credentials in 1Password again, with tags for the vaults that were originally used in Proton Pass and a tag "Imported $DATE" for later reference. I made a secure note with that date in the title in case I forget where it came from, and now I can move on with my life of only using one password manager at a time.
