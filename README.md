# LetSolo macOS

Homebrew casks and macOS application releases from LetSolo.

## Install applications

After this repository is published and a real cask has been added:

```bash
brew tap letsolo/macos
```

Install a specific app using its documented cask token. No placeholder LBV cask or installation URL is included here.

## Publisher identity

All commits and pushes must use the organization-owned GitHub App bot. Administrator accounts may manage repository settings and Actions secrets; they must not author, commit, or push repository code.

Configure a checkout with the App's private credential configuration outside this repository:

```bash
python3 -I scripts/bot.py --config /absolute/path/to/private/app.json configure
```

This configures only this checkout: bot author and committer identity, commit and push hooks, and a GitHub App installation-token credential helper. It clears inherited GitHub credential helpers for this repository and disables inherited personal commit signing. Tokens are generated when needed and are not persisted. Authentication failure stops the operation instead of falling back to a personal account.

After configuration, ordinary `git commit` and `git push` use the bot. Environment overrides to author or committer are checked by the hooks. Do not bypass hooks or replace this checkout's authentication settings.

To run `gh` as the bot without switching any global account:

```bash
git bot-gh release list --repo letsolo/homebrew-macos
```

To verify the current remote default-branch commit belongs to the bot:

```bash
python3 -I scripts/bot.py --config /absolute/path/to/private/app.json verify
```

## Actions configuration

Set these values before the first bot push:

- Variable `PUBLISHER_APP_CLIENT_ID`: the GitHub App client ID.
- Secret `PUBLISHER_APP_PRIVATE_KEY`: the App private key.

The `letsolo-vapor` App is shared with `letsolo/windows`. Install it on those two selected repositories with Contents write and Workflows write. Each installation token is scoped to the current repository. Workflows write is required when the bot creates or changes workflow files. Do not substitute a personal token.

The `Publisher identity` workflow checks the author, committer, and push actor. It does not create application releases or grant a person permission to submit code. Branch protection is a separate repository setting; local hooks can be bypassed deliberately and are not a server-enforced access policy.
