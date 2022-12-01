+++
title = "ZSH Config"
author = ["Ved Evilolive"]
description = "ZSH lit config for zinit"
date = 2022-11-30T23:54:00-07:00
draft = false
toc = true
+++

## Goals {#goals}

-   [ ] Move things like exa/fd/etc... to asdf and base this around that?


## Profiling {#profiling}

Sometimes I wanna test it outside of just zinit's stuff

```shell
# zmodload zsh/zprof
```


## P10K Instant Prompt {#p10k-instant-prompt}


### ~~Instant Prompt~~ {#ca9145}

**Currently disabled because of how I want to do my login thing. But zinit seems fast enough it's fine.**
Enables Powerlevel10k instant prompt. Should stay close to the top of ~/.zshrc.
Initialization code that may require console input (password prompts, [y/n]
confirmations, etc.) must go above this block; everything else may go below.

```shell
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
    source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi
```


## ZINIT Bootstrap {#zinit-bootstrap}

```shell
if [[ ! -f $HOME/.local/share/zinit/zinit.git/zinit.zsh ]]; then
    print -P "%F{33} %F{220}Installing %F{33}ZDHARMA-CONTINUUM%F{220} Initiative Plugin Manager (%F{33}zdharma-continuum/zinit%F{220})…%f"
    command mkdir -p "$HOME/.local/share/zinit" && command chmod g-rwX "$HOME/.local/share/zinit"
    command git clone https://github.com/zdharma-continuum/zinit "$HOME/.local/share/zinit/zinit.git" && \
        print -P "%F{33} %F{34}Installation successful.%f%b" || \
        print -P "%F{160} The clone has failed.%f%b"
fi
source "$HOME/.local/share/zinit/zinit.git/zinit.zsh"
autoload -Uz compinit
compinit
```


## Zinit Module {#zinit-module}

```shell
#module_path+=( "/Users/kailash/.zinit/bin/zmodules/Src" )
#zmodload zdharma/zplugin
```


## Keybinds {#keybinds}

First get my emacs style ones back.

```shell
zinit snippet OMZL::key-bindings.zsh
```

Now let's unfuck Atuin's stuff

```shell
bindkey "^[^[[C" forward-word     # alt+right to move forward one word
bindkey "^[^[[D" backward-word    # alt+left to move backward one word
bindkey '^[^?' backward-kill-word  # alt+delete to backward delete word
```


## Powerlevel10K {#powerlevel10k}

```shell
#zinit ice depth"1"
#zinit light romkatv/powerlevel10k
zinit depth"1" lucid nocd for \
  romkatv/powerlevel10k
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```


## Popular Annexes {#popular-annexes}

```shell
zinit light-mode for \
    zdharma-continuum/zinit-annex-as-monitor \
    zdharma-continuum/zinit-annex-bin-gem-node \
    zdharma-continuum/zinit-annex-patch-dl \
    zdharma-continuum/zinit-annex-submods \
    zdharma-continuum/zinit-annex-rust
```


## iTerm2 {#iterm2}

```shell
if [ "$TERM_PROGRAM" = "iTerm.app" ]; then
    test -e "${HOME}/.iterm2_shell_integration.zsh" && source "${HOME}/.iterm2_shell_integration.zsh"
fi
```


## Term stuff {#term-stuff}

```shell
zinit ice wait lucid
zinit snippet OMZP::term_tab
```


## Stylua {#stylua}

```shell
# zinit ice lucid wait'1c' id-as'stylua-cargo' as'command' pick'bin/stylua' cargo'!stylua'
# zinit light zdharma-continuum/null
```


## Options {#options}

```shell
setopt AUTO_MENU
setopt AUTO_CD
setopt AUTO_PUSHD
setopt CDABLE_VARS
setopt PUSHD_IGNORE_DUPS
setopt BANG_HIST
setopt COMPLETE_IN_WORD
setopt HIST_EXPIRE_DUPS_FIRST
setopt HIST_FIND_NO_DUPS
setopt HIST_IGNORE_ALL_DUPS
setopt HIST_IGNORE_DUPS
setopt HIST_IGNORE_SPACE
setopt HIST_NO_FUNCTIONS
setopt HIST_REDUCE_BLANKS
setopt HIST_SAVE_NO_DUPS
setopt SHARE_HISTORY
setopt EXTENDED_HISTORY
setopt INC_APPEND_HISTORY
setopt APPEND_HISTORY
COMPLETION_WAITING_DOTS="true"
unsetopt BEEP

```


### Completion options {#completion-options}

```shell
zstyle ':completion:*' menu select
```


## Aliases {#aliases}

```shell
{{- if (and .is_macos (lookPath "ggrep")) }}
alias grep={{ lookPath "ggrep" | quote }}
alias sgrep="/usr/bin/grep"
{{- end }}
{{- if lookPath "dog" }}
alias dig={{ lookPath "dog" | quote }}
alias sdig={{ lookPath "dig" | quote }}
{{- end }}
{{- if lookPath "fd" }}
alias fd={{ cat (lookPath "fd") "--hidden --color always" | quote }}
{{- end }}
alias ds="~/.emacs.d/bin/doom sync"
alias ddr="~/.emacs.d/bin/doom doctor"
{{- if lookPath "nvim" }}
alias vim="nvim"
alias svim={{- lookPath "vim" | quote }}
{{- end }}
{{- if lookPath "gsed" }}
alias sed={{- lookPath "gsed" | quote }}
alias ssed={{- lookPath "sed" | quote }}
{{- end }}
```


### Chezmoi {#chezmoi}

```shell
alias cm={{ .chezmoi.executable | quote}}
alias cma="{{ .chezmoi.executable }} apply"
alias cmp="{{ .chezmoi.executable }} add"
alias cms="{{ .chezmoi.executable }} status"
alias cme="{{ .chezmoi.executable }} edit"
alias cmd="{{ .chezmoi.executable }} diff"
```


### Work {#work}

```shell
{{- if .workmode }}
alias gerpom="git push origin HEAD:refs/for/master"
alias updategerrit="git commit --amend --no-edit && gerpom"
alias cbblr="/usr/bin/ssh cobbler"
alias arsenal="arsenal --conf ~/.arsenal/arsenal.ini"
{{- end }}
```


## Pretty things {#pretty-things}

```shell

boxenfortune() {
  fortune -a futurama | lolcat -t | boxen --padding .5 --center --border-color=blue --background-color=black  --border-style=doubleSingle --align=center
}

boxenpony() {
frypic="bestest_fry.pony"
zoidberg="zoidberg.pony"
bender="bender.pony"
imgchoices=(${frypic} ${zoidberg} ${bender})
  typeset PONYSAY_TRUNCATE_HEIGHT=y
  typeset PONYSAY_TRUNCATE_WIDTH=y
  ponysay -o -f ${HOME}/chezmoi/misc/$(shuf -e -n 1 ${imgchoices}) | boxen --center --align=center --dim-border --border-style=doubleSingle --margin=0 --padding=0 --border-color=magenta
}

forpony() {
  # Print a random, hopefully interesting, adage.
if [[ $+commands[fortune] ]] && [[ $+commands[boxen] ]] && [[ $+commands[ponysay] ]]; then
#    fortune futurama | cowsay -f vader-koala -W 72 | lolcat -F 0.75
    # fortune futurama | ponysay -bunicode
    # fortune futurama | pokemonsay
    boxenfortune
    boxenpony
else
    echo "Get to installing fortune, boxen, and ponysay dammit"
fi
}

boxenascii() {
    susfry="fry.ascii"
    zoidberg="zoidberg.ascii"
    bender="bender.ascii"
    imgchoices=(${susfry} ${zoidberg} ${bender})
    cat ${HOME}/chezmoi/misc/$(shuf -e -n 1 ${imgchoices}) | boxen --center --align=center --dim-border --border-style=doubleSingle --margin=0 --padding=0 --border-color=magenta
}
```


## Source scripts {#source-scripts}


### Conda <span class="tag"><span class="work">work</span></span> {#conda}

```shell
{{- if (eq .chezmoi.hostname "bburks-mbp") }}
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$(CONDA_REPORT_ERRORS=false '/Users/bburks/opt/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
 eval "$__conda_setup"
 else
if [ -f "/Users/bburks/opt/miniconda3/etc/profile.d/conda.sh" ]; then
     . "/Users/bburks/opt/miniconda3/etc/profile.d/conda.sh"
else
     #export PATH="/Users/bburks/opt/miniconda3/bin:$PATH"
    export PATH="$PATH:/Users/bburks/opt/minicoda3/bin"
fi
fi
unset __conda_setup
# <<< conda initialize <<<
export DEFAULT_CONDA_ENV=spotx_release_python3
{{- end }}
```


### Cargo {#cargo}

This script puts it in the path, so not needed to add it manually.

```shell
if [ -f $HOME/.cargo/env ]; then
    source $HOME/.cargo/env
fi
```


### Google SDK {#google-sdk}

```shell
# The next line updates PATH for the Google Cloud SDK.
if [ -f '{{ .gcloudsdk }}/path.zsh.inc' ]; then . '{{ .gcloudsdk }}/path.zsh.inc'; fi

# The next line enables shell command completion for gcloud.
if [ -f '{{ .gcloudsdk }}/completion.zsh.inc' ]; then . '{{ .gcloudsdk }}/completion.zsh.inc'; fi
```


## Misc {#misc}


### Autoload/Complete {#autoload-complete}

```shell
autoload -U +X bashcompinit && bashcompinit
{{ if (lookPath "terraform") -}}
complete -o nospace -C {{ lookPath "terraform" }} terraform
{{- end }}
{{ if (lookPath "vault") -}}
complete -o nospace -C {{ lookPath "vault" }} vault
{{- end }}
```


### McFly {#mcfly}

Install. The config options are in zshenv which I need to move here.
This is the original way. Not using it in favor of switching to proper cargo and/or asdf


#### Deprecated {#deprecated}

```shell
zinit ice lucid wait"0a" from"gh-r" as"program" atload'eval "$(mcfly init zsh)"'
zinit light cantino/mcfly
```


#### Current {#current}

```shell
if (( $+commands[mcfly] )); then
    eval "$(mcfly init zsh)"
fi
```


## FZF {#fzf}

First install it

```shell
zinit ice from"gh-r" as"completion"; zinit light junegunn/fzf
```


### Now activate it {#now-activate-it}

```shell
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
```


### FZF Tabs {#fzf-tabs}

```shell
#zinit ice wait'2' lucid blockf compile'lib/*f*~*.zwc'
#zinit light Aloxaf/fzf-tab
```


### FZF Tab Settings/Completion {#fzf-tab-settings-completion}

```shell
autoload -Uz zstyle+
# disable sort when completing `git checkout`
zstyle ':completion:*:git-checkout:*' sort false
# set descriptions format to enable group support
zstyle ':completion:*:descriptions' format '[%d]'
# set list-colors to enable filename colorizing
zstyle ':completion:*' list-colors ${(s.:.)LS_COLORS}
# preview directory's content with exa when completing cd
#zstyle ':fzf-tab:complete:cd:*' fzf-preview 'exa -1 --color=always $realpath'
zstyle ':completion:*:*:cd:*:directory stack' menu yes select
# switch group using `,` and `.`
#zstyle ':fzf-tab:*' switch-group ',' '.'
zstyle+ ':completion:*'   list-separator '→' \
      + ''                completer _complete _match _list _prefix _ignored _correct _approximate _oldlist \
      + ''                group-name '' \
      + ''                use-cache on \
      + ''                verbose yes \
      + ''                rehash true \
      + ''                squeeze-slashes true \
      + ''                accept-exact '*(N)' \
      + ''                ignore-parents parent pwd \
      + ''                matcher-list 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=*' 'l:|=* r:|=*' \
      + ':manuals'        separate-sections true \
      + ':matches'        group 'yes' \
      + ':functions'      ignored-patterns '(_*|pre(cmd|exec))' \
      + ':approximate:*'  max-errors 1 numeric \
      + ':match:*'        original only \
      + ':messages'       format ' %F{purple} -- %d --%f' \
      + ':warnings'       format ' %F{1}-- no matches found --%f' \
      + ':corrections'    format '%F{5}!- %d (errors: %e) -!%f' \
      + ':default'        list-prompt '%S%M matches%s' \
      + ':descriptions'   format '[%d]' \
      + ':options'        description yes \
      + ':-tilde-:*'      group-order named-directories path-directories users expand \
      + ':cd:*'           tag-order local-directories directory-stack path-directories \
      + ':cd:*'           group-order local-directories path-directories \
      + ':git-checkout:*' sort false \
      + ':exa'            file-sort modification \
      + ':exa'            sort false \
      + ':sudo:*'         command-path /usr/local/sbin /usr/local/bin /usr/sbin /usr/bin /sbin /bin \
      + ':sudo::'         environ PATH="$PATH" \
      + ':*:zcompile:*'   ignored-patterns '(*~|*.zwc)' \
      + ':(rm|rip|kill|diff|git-dsf):*' ignore-line other \
      + ':(rm|rip|git-dsf):*'           file-patterns '*:all-files' \
      + ':complete:-command-::commands' ignored-patterns '*\~' \
      + ':*:-subscript-:*'              tag-order indexes parameters \
      + ':*:-redirect-,2>,*:*'          file-patterns '*.log'
```


### FZF Specific options {#fzf-specific-options}

```shell
export FZF_COMPLETION_OPTS="--filepath-word --height 40% --layout=reverse --color 'fg:#bbccdd,fg+:#ddeeff,bg:#334455,preview-bg:#223344,border:#778899' --border --info=inline --ansi"
```


#### FZF use FD for find {#fzf-use-fd-for-find}

```shell
export FZF_DEFAULT_COMMAND='fd --hidden'
```

The following sets it up to go on the base paths

```shell
_fzf_compgen_path() {
fd --follow --one-file-system --hidden --exclude ".git" . "$1"
}

# Use fd to generate the list for directory completion
_fzf_compgen_dir() {
fd --type d --follow --one-file-system --hidden --exclude ".git" . "$1"
}
```


## OMZ Plugins {#omz-plugins}

```shell
zinit ice wait lucid for \
    OMZL::completion.zsh \
    OMZL::termsupport.zsh \
    OMZL::history.zsh
zinit wait'0b' lucid for \
    OMZP::helm \
    OMZP::oc \
    OMZP::kubectl \
    OMZP::minikube \
    OMZP::fasd \
    OMZP::git
if (( $+commands[thefuck] )); then
    zinit snippet OMZP::thefuck
fi
```


## EXA {#exa}

```shell
zinit ice wait'0c' lucid id-as'exa' from'github' as'completion' cp'completions/zsh/_exa -> _exa' depth'1' \
  atload"
        alias ls='exa -a --icons -F --time-style long-iso --git --color=auto'
        alias l='exa --sort=changed --icons -la --git --git-ignore --ignore-glob=\".DS_Store|__MACOSX|__pycache__\"'
        alias la='exa --group-directories-first --icons -la'
        alias ll='exa --group-directories-first --icons -la --color-scale --time-style=long-iso --git --git-ignore --ignore-glob=\".git|.DS_Store|__MACOSX|__pycache__\" -T -L2'
        alias ll3='exa --group-directories-first --icons -la --git --git-ignore --ignore-glob=\".git|.DS_Store|__MACOSX\" -T -L3'
        alias ll4='exa --group-directories-first --icons -la --git --git-ignore --ignore-glob=\".git|.DS_Store|__MACOSX\" -T -L4'
        alias tree='exa --group-directories-first -T --icons'
    "
zinit light ogham/exa
```


## Bat {#bat}

<https://github.com/sharkdp/bat>


### Install {#install}

```shell
zinit ice lucid as'completion' id-as'bat' from'gh-r' cp'bat/autocomplete/bat.zsh -> _bat' atload'alias cat=bat;alias scat=/bin/cat' wait
zinit light sharkdp/bat
```


### Use with man {#use-with-man}

```shell
export MANPAGER="sh -c 'col -bx | bat -l man -p'"
# MANROFFOPT="-c"  if formatted weird
```


## Ripgrep {#ripgrep}

```shell
#zinit ice lucid wait from'gh-r' as'completion' id-as'rg' cp'rg/complete/_rg -> _rg'
#zinit light BurntSushi/ripgrep
```


## FD {#fd}

```shell
zinit as="null" lucid from="gh-r" for \
    mv="fd* -> fd"   sbin="fd/fd"  @sharkdp/fd \
    cp="ripgrep*/complete/_rg -> _rg"   sbin="ripgrep* -> rg" @BurntSushi/ripgrep
```


### Straight method {#straight-method}

```shell
zinit ice as'program' id-as'fd' from'gh-r' mv'fd* -> fd' lucid
zinit light sharkdp/fd
```


### Cargo method {#cargo-method}

```shell
zinit wait='[[ -v CARGO_HOME && -v RUSTUP_HOME ]]' for \
    id-as'rust-fd' cargo'!fd-find' lucid\
        zdharma-continuum/null
```


## Direnv {#direnv}

This broke for some reason, so doing it the old way


### Broken {#broken}

```shell
#zinit from"gh-r" as"completion" mv"direnv* -> direnv" \
#    atclone'./direnv hook zsh > zhook.zsh' atpull'%atclone' \
#    pick"direnv" src="zhook.zsh" for \
#        direnv/direnv
```


### Working {#working}

Requires installed via brew or something

```shell
eval "$(direnv hook zsh)"
```


## Misc MacOS {#misc-macos}

```shell
{{- if .is_macos }}
zinit ice lucid svn
zinit snippet OMZP::macos
zinit ice lucid wait as'completion'
zinit snippet OMZP::brew
{{- end }}
```


## ZSH Autopair {#zsh-autopair}

```shell
zinit ice wait'3' lucid
zinit light hlissner/zsh-autopair
```


## PIP {#pip}

```shell
zinit ice wait'2' lucid
zinit snippet OMZP::pip
zinit ice as'completion'; zinit snippet OMZP::pip/_pip
```


## Function to Tangle from CLI {#function-to-tangle-from-cli}

```shell
function tangle-file() {
 emacs --batch -l org $@ -f org-babel-tangle
}
```


## ASDF {#asdf}

Decided to stop installing it, thought it might be causing issues? So just switched to native install. Shims on shims probably isn't good. But this doesn't actually seem to do anything either.
This is now installed via git for all OS's to keep the path work sane

```shell
# zinit ice ver'v0.9.0' src'asdf.sh' mv'_asdf -> _asdf' nocompile
#zinit ice ver'v0.9.0' pick'asdf.sh' fpath'completions'
#zinit light asdf-vm/asdf
# zinit ice wait'3' lucid
#source $(brew --prefix asdf)/libexec/asdf.sh
{{- if not .chez.caelus }}
[[ -f ${HOME}/.asdf/asdf.sh ]] && . ${HOME}/.asdf/asdf.sh
{{- end }}
{{ if .chez.caelus -}}
[[ -f $(brew --prefix asdf)/libexec/asdf.sh ]] && source $(brew --prefix asdf)/libexec/asdf.sh
{{- end }}
zinit ice as'completion' lucid
zinit snippet OMZP::asdf/asdf.plugin.zsh
```


## Haskell {#haskell}

```shell
[ -f "$HOME/.ghcup/env" ] && source "$HOME/.ghcup/env"
```


## Nix {#nix}

```shell
[ -f $HOME/.nix-profile/etc/profile.d/hm-session-vars.sh ] && source $HOME/.nix-profile/etc/profile.d/hm-session-vars.sh
```


## Salt {#salt}

Completion

```shell
zinit ice as'completion'
zinit snippet OMZP::salt/_salt
```


## Forgit {#forgit}

```shell
zinit ice wait lucid
zinit light 'wfxr/forgit'
```


## Command-not-found {#command-not-found}

Suggests things if you don't have it installed. But does need a helper. On macOS it's `brew tap homebrew/command-not-found`

```shell
zinit snippet OMZP::command-not-found
```


## SSH {#ssh}

Sets up the agent, hopefully unfucking the macos stuff

```shell
zinit ice wait'0c' lucid
zinit snippet OMZP::ssh-agent
#zinit snippet PZTM::ssh
mykeyids=("id_ed25519")
case $(hostname -s) in
    "bburks-mbp")
        keyids+=("magnite-github", "id_ecdsa_sk")
        ;;
    "caelus"|"coeus"|"frontiere")
        [[ -f "${HOME}/.ssh/vault-signed.pub" ]] && keyids+=("vault-signed.pub")
        ;;
esac
for akey in ${mykeyids}; do
    mykeypaths+="${HOME}/.ssh/${akey}"
done
zstyle ':omz:plugins:ssh-agent' identities ${mykeyids}
```


## zoxide {#zoxide}

```shell
#zinit ice wait"0a" as"command" from"gh-r" lucid \
#  atclone"./zoxide init zsh > init.zsh" \
#  atpull"%atclone" src"init.zsh" sbin'zoxide!' nocompile'!'
#zinit light ajeetdsouza/zoxide
zinit ice wait'3c' lucid
zinit snippet OMZP::zoxide
```


## chezmoi completion {#chezmoi-completion}

```shell
zinit ice as"completion" from"gh-r" id-as"chezmoi" \
    mv"completions/chezmoi.zsh -> _chezmoi" \
    pick"_chezmoi" \
    atpull"!git reset --hard"
zinit light twpayne/chezmoi
# chezmoi completion zsh --output=$HOME/.local/share/zinit/completions/_chezmoi
```


## SOPS {#sops}

```shell
zinit ice wait'1a' from"gh-r" as"completion" sbin"sops* -> sops" bpick"*.{{.chezmoi.os}}" nocompile lucid
zinit light mozilla/sops
#zinit ice lucid from"gh-r" as'completion'; zinit light mozilla/sops
```


## Tmux {#tmux}

Now for the generic install/settings. But we'll skip it if no tmux.

```shell
{{- if lookPath "tmux" }}
zinit ice lucid
zinit snippet OMZP::tmux
{{- end }}
```


## Ubuntu {#ubuntu}

```shell
{{- if eq .chezmoi.os "linux" -}}
{{- if eq .chezmoi.osRelease.id "ubuntu" }}
zinit ice wait'0c' lucid
zinit snippet OMZP::ubuntu
{{- end }}
{{- end }}
```


## Path Changes {#path-changes}


### Local bin in home {#local-bin-in-home}

```shell
[[ -d "${HOME}/.local/bin" ]] && export PATH=$PATH:$HOME/.local/bin
```


### lua-language-server {#lua-language-server}

```shell
[[ -d "${HOME}/.local/share/tools/lua-language-server/darwin/bin/lua-language-server" ]] && export PATH=$PATH:$HOME/.local/share/tools/lua-language-server/darwin/bin
```


## Lazygit {#lazygit}

Looks neat

```shell
zinit ice lucid wait'0b' as'null' from'gh-r' sbin'lazygit' atload'alias lg=lazygit'
zinit light 'jesseduffield/lazygit'
```


## Dog {#dog}

Great DNS tool

```shell
zinit ice wait"0a" from"gh-r" as"null" sbin"bin/dog" mv'completions/dog.zsh -> _dog' lucid
zinit light ogham/dog
```


## Kitty {#kitty}

```shell
if (($+commands[kitty])); then
# zinit ice lucid
zinit ice id-as'kitty' cp'functions/_kitty_complete -> _kitty'
zinit light redxtech/zsh-kitty
fi
```

```shell
if test -n "$KITTY_INSTALLATION_DIR"; then
  export KITTY_SHELL_INTEGRATION="no-cursor"
  autoload -Uz -- "$KITTY_INSTALLATION_DIR"/shell-integration/zsh/kitty-integration
  kitty-integration
  unfunction kitty-integration
fi

if [[ ! -z "${KITTY_WINDOW_ID}" ]]; then
  kitty + complete setup zsh | source /dev/stdin
fi
```


## Pet {#pet}

CLI snippets

```shell
zinit ice lucid as"null" from"gh-r" sbin"pet" mv"misc/completions/zsh/_pet -> _pet"
zinit light knqyf263/pet
```


## Poetry Completions {#poetry-completions}

```shell
zinit ice lucid pick'poetry.zsh'
zinit light sudosubin/zsh-poetry
```


## Completions {#completions}

```shell
zinit ice wait'0b' lucid depth=1; zinit light 3v1n0/zsh-bash-completions-fallback

zinit wait'1a' lucid light-mode for \
  ver'develop' atinit'ZSH_AUTOSUGGEST_BUFFER_MAX_SIZE=20' atload'_zsh_autosuggest_start' \
    zsh-users/zsh-autosuggestions \
  is-snippet atload'zstyle ":completion:*" special-dirs false' \
    PZTM::completion \
  atload"zicompinit; zicdreplay" blockf \
    zsh-users/zsh-completions \
  atinit'zicompinit; zicdreplay' atload'FAST_HIGHLIGHT[chroma-man]=' \
    atclone'(){local f;cd -q →*;for f (*~*.zwc){zcompile -Uz -- ${f}};}' \
    compile'.*fast*~*.zwc' nocompletions atpull'%atclone' \
        zdharma-continuum/fast-syntax-highlighting \
  atload'bindkey "^[[A" history-substring-search-up;
    bindkey "^[[B" history-substring-search-down' \
        zsh-users/zsh-history-substring-search
  #blockf atpull'zinit creinstall -q .' \
  #  zdharma-continuum/fast-syntax-highlighting
autoload -Uz _zinit
(( ${+_comps} )) && _comps[zinit]=_zinit

zinit cdreplay -q
```


## Stop Profiling {#stop-profiling}

```shell
# zprof
```
