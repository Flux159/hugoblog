---
title: GPG Signing Github Commits
date: 2025-06-29
categories:
  - Experiments
tags:
  - experiments
  - gpg
  - cryptography
  - codesigning
---

Recently I had to get GPG signing of my commits on Github to contribute to a repository. Documenting the process in a single post since Github's docs are pretty verbose on this.

## GPG Signing Github commits

### Installation (Mac)

Install Homebrew: https://brew.sh/ 

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Get gnupg via Homebrew: https://formulae.brew.sh/formula/gnupg 

```shell
Brew install gnupg
```

### Installation (Ubuntu)

On Ubuntu (tested with 24.04):
```shell
apt-get update && apt-get install -y gpg
```

### Creating the key

Creating a key (full docs here):

https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key 

```shell
gpg --full-generate-key
```

Use RSA (3072 or 4096), default for most other things. You may need to pass in exact name & exact email that you used on Github. 

It will get you to type a passphrase as part of this process. 

On Mac OS, the passphrase seemed to already be setup to use keychain so I didn’t need to type it in everytime I signed GitHub commits, but you may need to do this manually (my `git config --list` shows it’s setup to use keychain).

### Listing your keys

List keys (full docs here):

https://docs.github.com/en/authentication/managing-commit-signature-verification/checking-for-existing-gpg-keys 

```shell
gpg --list-secret-keys --keyid-format=long
```

Get the public key (key_id is the id AFTER the “sec   rsa3072/“ part from the list keys above):
```shell
gpg --armor --export <key_id>
```

Add key to your Github account (full docs here):
 
https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account 

From that export command, go here: https://github.com/settings/keys and then add a new GPG key adding your public key (including the `-----BEGIN PGP PUBLIC KEY BLOCK-----` and `-----END PGP PUBLIC KEY BLOCK-----` parts)

### Using with git

When you make a commit, use the `-S` flag, you will sign the commit. Example:
```shell
git commit -S -m "YOUR_COMMIT_MESSAGE"
```

Git push to push commit to remote:
```shell
git push 
```

### Using with sapling

Note that I had issues with this with interactive smartlog & trying to get it working via CLI (the passphrase thing didn't work correctly). It's relatively new for sapling so kinda buggy.

Full docs: https://sapling-scm.com/docs/git/signing/ 

Check that your sapling username and email are set to the same thing as git:
```shell
sl config ui.username
```

Configure sapling to sign commits (key_id is the same as above):
```shell
sl config --local gpg.key <key_id>
```
You should see: “updated config in `<repo_dir>/.sl/config`”

Note that there’s no need to use a -S flag as it’s configured for all sapling commits in a specific repository.

If you want for all repositories, you want to use:
```shell
sl config —user gpg.key <key_id>
```
You should see “updated config in `~/Library/Preferences/sapling/sapling.conf`”

Disabling for sapling incase it doesn't work correctly:
```shell
# User
sl config --user gpg.enabled=false
# Local
sl config --local gpg.enabled=false
```

### Misc

Copying keys to another computer (just copy the entire ~/.gnupg directory over w/ scp, etc.): 
https://superuser.com/questions/1277421/copying-gnupg-keys-to-a-new-computer
