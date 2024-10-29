# Bash Terminal

![Bash](../assets/bash-logo.png?raw=true)

Bourne Again SHell ([Bash](https://www.gnu.org/software/bash/)) is the GNU Project's shell.

Bash is an sh-compatible shell that incorporates useful features from the Korn shell (ksh) and C shell (csh).

## Terminal theme

The standard macOS Terminal appearance is just boring. Apple included a few nice preset themes too, but to really make your terminal appearance stand out you need to customize it yourself.

Below is the Terminal using my personal theme and `.bash_profile` configuration. Feel free to copy and use it.

![macOS Bash theme](../assets/terminal-bash.png?raw=true)

### Enable Antialias text, Bold Fonts, ANSI Colors, and Bright Colors

1. Open the macOS Terminal
2. In the top menu bar, select `Terminal` then `Preferences...` (⌘,)
3. Select the `Profiles` tab
4. Choose the **Basic** or **Pro** profile/theme from the left side list
5. Press the `Default` button near the bottom of the window
6. Under the `Text` tab check the boxes:

   * `Antialias text`
   * `Use bold fonts`
   * `Display ANSI colors`
   * `Use bright colors for bold text`

![Terminal Profiles - Text](../assets/terminal-profiles-text.png?raw=true)

## Update and select Bash as the default shell

You can update and set up Bash following this tutorial or using the mac-setup install script:

```bash
bash <(curl -fsSL raw.githubusercontent.com/monsieurborges/mac-setup/master/install) bash
```

If you decide to do it on your own:

1. Define HOMEBREW_PREFIX

    ```bash
    if [[ "$(/usr/bin/uname -m)" == "arm64" ]]; then
        HOMEBREW_PREFIX="/opt/homebrew"
    else
        HOMEBREW_PREFIX="/usr/local"
    fi

    eval "$(${HOMEBREW_PREFIX}/bin/brew shellenv)"
    ```

2. Update bash:

    ```bash
    brew install bash
    ```

3. Add the new shell to the list of allowed shells:

    ```bash
    if $(grep -Fxq "${HOMEBREW_PREFIX}/bin/bash" /etc/shells); then
        echo "The list of shells is already updated"
    else
        sudo echo "${HOMEBREW_PREFIX}/bin/bash" >> /etc/shells
    fi
    ```

4. Change to the new shell:

    ```bash
    chsh -s "${HOMEBREW_PREFIX}/bin/bash"
    ```

5. Check the bash version:

    ```bash
    bash --version
    ```

## Improve Bash theme

![Bash command line history](../assets/terminal-bash.gif?raw=true)

Follow the tutorial or just paste that on the Terminal:

```bash
bash <(curl -fsSL raw.githubusercontent.com/monsieurborges/mac-setup/master/install) bashconfig
```

### Set up the command line history search

Paste that in the terminal:

```bash
read -r -d '' INPUTRC <<"EOF"
"\e[A":history-search-backward
"\e[B":history-search-forward

set colored-stats on
set mark-symlinked-directories on
set show-all-if-ambiguous on
set show-all-if-unmodified on
set visible-stats on
set completion-ignore-case on
TAB: menu-complete
EOF

echo "${INPUTRC}" > "${HOME}/.inputrc"
```

### Update `.bash_profile`

1. Backup the current `.bash_profile` file:

    ```bash
    if [[ -f "${HOME}/.bash_profile" ]]; then
        cp "${HOME}/.bash_profile" "${HOME}/.bash_profile.bkp"
    fi
    ```

2. Update the `.bash_profile`. Paste that in the terminal:

    ```bash
    read -r -d '' BASH_PROFILE <<"EOF"
    #!/bin/bash
    # Bash Profile for macOS
    # Copyright (c) Monsieur Borges
    # https://github.com/monsieurborges/mac-setup

    # LS Colors
    # CLICOLOR use ANSI color sequences to distinguish file types
    export CLICOLOR=true
    export LSCOLORS=gxegbxdxcxahadabafacge
    alias ls='ls -GFh'

    # Bash Colors and formatting
    CYAN="\[\e[38;5;6m\]"
    MAGENTA="\[\e[38;5;13m\e[1m\]"
    SKYBLUE="\[\e[38;5;25m\e[1m\]"
    NONE="\[\e[0m\e[21m\]"

    # Prevent Mac OS ._ in in tar.gz files
    export COPYFILE_DISABLE=true

    # Homebrew
    if [[ "$(/usr/bin/uname -m)" == "arm64" ]]; then
        # ARM macOS
        HOMEBREW_PREFIX="/opt/homebrew"
    else
        # Intel macOS
        HOMEBREW_PREFIX="/usr/local"
    fi

    eval "$(${HOMEBREW_PREFIX}/bin/brew shellenv)"

    # Homebrew completion
    if [[ -f "$(brew --prefix)/etc/bash_completion.d/brew" ]]; then
        source "$(brew --prefix)/etc/bash_completion.d/brew"
    fi

    # Bash completion@2
    if [[ -r "$(brew --prefix)/etc/profile.d/bash_completion.sh" ]]; then
        source "$(brew --prefix)/etc/profile.d/bash_completion.sh"
    fi

    # Bash-Git-prompt
    if [[ -f "$(brew --prefix)/etc/bash_completion.d/git-prompt.sh" ]]; then
        GIT_PS1_SHOWCOLORHINTS=true
        GIT_PS1_SHOWDIRTYSTATE=true
        GIT_PS1_SHOWUNTRACKEDFILES=true
        GIT_PS1_DESCRIBE_STYLE='default'
        source "$(brew --prefix)/etc/bash_completion.d/git-prompt.sh"
    fi

    # exa is a replacement for ls https://github.com/ogham/exa
    if command -v exa 1>/dev/null 2>&1; then
        alias ls="exa --group-directories-first --classify"
    fi

    # Python virtual environment
    # Allow pip only for active virtual environment
    # Use 'gpip' for global environment
    export PIP_REQUIRE_VIRTUALENV=true
    gpip(){
        PIP_REQUIRE_VIRTUALENV="" pip "${@}"
    }

    # Pyenv Python version management
    if command -v pyenv 1>/dev/null 2>&1; then
        eval "$(pyenv init -)"
        pyenv virtualenvwrapper
    fi

    # PRIMARY PROMPT
    PROMPT_COMMAND='__git_ps1\
                    "\n${MAGENTA}[\d \t] ${SKYBLUE}$(python --version 2>&1)${NONE}\
                    \n\u@\h: ${CYAN}\w${NONE}"\
                    "\n${VIRTUAL_ENV:+($(basename ${VIRTUAL_ENV}))}\\$ "\
                    " (%s)"'
    EOF

    echo "${BASH_PROFILE}" > "${HOME}/.bash_profile"
    ```

### Install bash-completion

[bash-completion](https://github.com/scop/bash-completion) is a collection of command line command completions for the Bash shell.

```bash
brew install bash-completion@2
```

### Install docker command-line completion (optional)

```bash
brew install docker-completion docker-machine-completion docker-compose-completion
```

### Install exa (optional)

[exa](https://the.exa.website) is a replacement for ls written in Rust.

```bash
brew install exa
```
