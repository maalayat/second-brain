- [[FZF]]

### Top 5

```
alias ..="cd .."
alias ...="cd ../.."
alias ll="exa -l"
alias la="exa -la"
alias cdc="cd '$HOME/code'"
alias o=xdg-open

alias gaa="git add -A"
alias gc="git commit"
alias gs="git status -sb"
alias gf="git fetch --all -p"
alias gps="git push"
alias gpsf="git push --force"
alias gpl="git pull --rebase --autostash"
alias gb="git branch"
```

- [Exa](https://github.com/ogham/exa) A modern replacement for ‘ls’. 

### Youtube-dl

Video download and extract audio
```
ydl='youtube-dl --merge-output-format mp4 -f '\''bestvideo+bestaudio[ext=m4a]/best'\'' --embed-thumbnail --add-metadata'
```

```
function ff() {
  ffmpeg -i $1 -vn -ar 44100 -ac 2 -b:a 192k ${1%.**}.mp3
}
```

## Styling the prompt

### Config

Adding this to zsh file:
```
setopt PROMPT_SUBST
```

### Exit code
```
function prompt_exit_code() {
  local EXIT="$?"

  if [ $EXIT -eq 0 ]; then
    echo -n green
  else
    echo -n red
  fi
}
```

### Git info
```
function git_prompt_info {
  inside_git_repo="$(git rev-parse --is-inside-work-tree 2>/dev/null)"

  if [ "$inside_git_repo" ]; then
    current_branch=$(git branch --show-current)
    print -P " on %{%F{yellow}%}$current_branch%{%f%}"
  else
    echo ""
  fi
}
```

### Setting the prompt
```
PROMPT='%{%F{$(prompt_exit_code)}%}%n%{%f%S}% @ %d$(git_prompt_info):'
PROMPT='%{%F{$(prompt_exit_code)}%}%n%{%f%} @ %d$(git_prompt_info): '
```