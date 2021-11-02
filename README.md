# Vizion

Vizion will provizion (🔥👌😂💯) a MacOS machine from scratch. This setup is what I like to call the holy trinity, since it includes:
 - [Oh My Zsh](https://ohmyz.sh/)
 - [Vim](https://www.vim.org/)
 - [Tmux](https://github.com/tmux/tmux)

Several other tools are also installed and configured such as:
 - [iTerm](https://www.iterm2.com/)

Provisioner will symlink dotfiles to the home directory, and will assume your coding projects live in `~/Developer/code`.

### Installation
Easy peasy lemon squeezy.

First generate a GitHub token here: https://github.com/settings/tokens

Then install the XCode Developer Tools:

```
xcode-select --install
```

Finally run:

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/christopher-francisco/vizion/master/install.sh)"
```

### Retting up GitHub Hub for Enterprise

The confusing part is the `GITHUB_HOST` vs the whitelisting with `git config --global --add hub.host MY.GIT.ORG`. It goes like this:

`hub` is able to infer what HOST it should use for git operations, from the `git remote` output. We only need to whitelist said HOST.

From this point onwards, for operations like `clone` or `init`, it will default to `github.com`. If we want to clone an Enterprise one, we need to run it like `GITHUB_HOST=my.github.org git clone user/repo`.

THE PROBLEM is that once that's done, operations like `git pull-request` or such requires the API token. If we use both Cloud & Enterprise, we'll need to switch the access token before operations.


So, instead of whitelisting, this is my approach:

```bash
useCloudGitHub() {
    export GITHUB_TOKEN=token
    export GITHUB_HOST=github.com
}
useEnterpriseGitHub() {
    export GITHUB_TOKEN=token
    export GITHUB_HOST=my.github.org
}
```

Then we just call the bash function as soon as we enter the repo. Since we're explicitly setting the `GITHUB_HOST`, we don't need to whitelist anything.

### Features
 - Complete support for true colors and italics on iTerm, iTerm+Vim, iTerm+Tmux, iTerm+Tmux+Vim.
 - Best mappings & plugins for Vim and Tmux

## Troubleshooting
>Hub returns *422 Invalid value for "base"* when running `hub pull-request`.

[As discussed in this issue](https://github.com/github/hub/issues/154#issuecomment-410277347), just set the base branch of the remote:

```bash
git remote set-head origin -a
```

> Tmux plugins won't load
[Install `gawk`](https://github.com/tmux-plugins/tpm/issues/146)
```bash
brew install gawk
```

## Known bugs and TODO
### Rakefile
Need a task to upgrade everything. Install should not fail because upgrading

### Vim
 - [ ] Fix problem with vim airlines trying to load before installing the plugin

### SSH
 - [ ] Add ssh keys to agent

### GPG
 - Copying the key to clipboard
```
$ gpg --list-secret-keys --keyid-format LONG
$ gpg --armor --export FINGERPRINT | pbcopy
```
