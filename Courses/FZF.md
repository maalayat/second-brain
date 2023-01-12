### Docker connect
```
alias dcon="_docker_connect"
_docker_connect() {
  containerid=$(docker ps | tail -n +2 | fzf | awk '{print $1}')
  docker exec -it $containerid bash
}
```

### Git clone
```
alias gcl="clone_git_repo"

clone_git_repo() {
  repo_url=$(curl -s -H "Authorization: token $GITHUB_TOKEN" "https://api.github.com/user/repos?per_page=200" | jq --raw-output ".[].ssh_url" | fzf)
  git clone "$repo_url"
  echo "$repo_url"
}
```

### AWS
```
alias awsssh="aws_ssh"

aws_ssh() {
  instancePublicIp="$(ec2_instance_public_ip)"
  xdotool key shift+F10 r 2
  ssh -i ~/.ssh/codely.pem ubuntu@$instancePublicIp
}

ec2_instance_public_ip() {
  local ip=$(aws ec2 describe-instances --output text --filters "Name=tag:Name,Values=*" --query "Reservations[*].Instances[*].{IP:PublicIpAddress,Name:Tags[?Key=='Name']|[0].Value}" | fzf | awk '{print $1}')
  echo "$ip"
}
```

### ls with super powers

```
_display_message() {
  # dirtomove=$(ls | fzf)
  dirtomove=$(ls -ad */ | fzf)
  cd "$dirtomove"
}

zle         -N    _display_message
bindkey  '^h'  _display_message
```

### reverse search with super powers

```
_reverse_search() {
  local selected_command=$(fc -rl 1 | awk '{$1="";print substr($0,2)}' | sort --unique | fzf)
  LBUFFER=$selected_command
}

zle -N _reverse_search
bindkey '^r' _reverse_search
```

## Community bindings
![[zsh_bindings_gce.png]]

![[zsh_bingings_dir.png]]

#### Open a project
```
_open_project(){

project=$(ls "$HOME/Documents/projects" | fzf)  
cd "$HOME/Documents/projects/$project"  
code .  
}  
zle -N _open_project  
bindkey "^k" _open_project
```

#### Killer
```
_kill_process() {
  pid=$(ps -aux | fzf | awk '{print $2}')
  kill -9 "$pid"
}


zle -N _kill_process
bindkey "^k" _kill_process
```

#### git branch
```
_change_branch() { branch_name=$(git branch | fzf);
  git checkout "${branch_name##*( )}";
}
```