# Git

![Git](../assets/git.png?raw=true)

[Git](https://git-scm.com) is a free and open source distributed version control system.

## Install and set up git

### Option 1 - Easy install using mac-setup:

```bash
bash <(curl -fsSL raw.githubusercontent.com/monsieurborges/mac-setup/master/install) git
```

### Option 2 - Install on your own:

#### Define HOMEBREW_PREFIX

```bash
if [[ "$(/usr/bin/uname -m)" == "arm64" ]]; then
    HOMEBREW_PREFIX="/opt/homebrew"
else
    HOMEBREW_PREFIX="/usr/local"
fi

eval "$(${HOMEBREW_PREFIX}/bin/brew shellenv)"
```

#### Install git

```bash
brew install git
```

#### Set git user name and email

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

#### Set git terminal colors

```bash
git config --global color.ui true
git config --global color.status.changed "blue normal"
git config --global color.status.untracked "red normal"
git config --global color.status.added "magenta normal"
git config --global color.status.updated "green normal"
git config --global color.status.branch "yellow normal bold"
git config --global color.status.header "white normal bold"
```

## Check Git configuration

Check your name and e-mail:

```bash
git config --global --list
```
