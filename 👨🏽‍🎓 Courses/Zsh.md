## iterm alternatives
- https://gnunn1.github.io/tilix-web/
- https://www.deepin.org/en/original/deepin-terminal/
- https://st.suckless.org/
- https://starship.rs/

## Other alternatives
- https://hyper.is/
- https://github.com/Eugeny/tabby
- https://github.com/alacritty/alacritty

## Setup
```
chsh -s $(which zsh)`
```

## Verify zsh instalation
```
echo $0  
/usr/bin/zsh
```

## Configuration files
- .zshrc: Personal configuration
- .zlogin
- .zshenv:

## Remove 'last login'
We need create a `.hushlogin` in the user directory

## Binding keys

### List all bindings

```
bindkey -l
```

### List main bindings

```
bindkey -M main
```

### How to build our own binding

```
_display_message() {
  echo -n "Message"
}

zle         -N    _display_message
bindkey  '^h'  _display_message
```

## Frameworks

### Oh My Zsh
- [ohmyz.sh](https://ohmyz.sh/)
- [Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)
- https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet

#### Plugins 
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

#### Default user

```
DEFAULT_USER=`whoami`
```

#### Profiling

##### Statistics

```
for i in $(seq 1 10); do time zsh -i -c exit; done
```

##### zprof

```
zmodload zsh/zprof

# ...

zprof
```

### ZIM
- [zimfw](https://github.com/zimfw/zimfw)

#### Theme

##### Edit .zimrc
```
zmodule <name_theme>
```
##### Apply theme
```
zimfw install
```

##### [Themes](https://github.com/zimfw/zimfw/wiki/)
- eriner

## Common aliases
- https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/common-aliases

## key codes table
- https://www.zsh.org/mla/users/2014/msg00266.html

## Lazy operations

Move to a file.sh the hard process for example: `hard_process.sh`

```
function hard_process {
  echo "Start!"
  sleep 2
  echo "Done!"
}
```

```
function hp {
  fname=${declare -f -F hard_process}

  [ -n "$fname" || source "$HOME/hard_process.sh" ]

  hard_process
}
```

## Resources
- [Configuring Zsh Without Dependencies](https://thevaluable.dev/zsh-install-configure-mouseless/)
- [Writing zsh completion scripts](https://blog.mads-hartmann.com/2017/08/06/writing-zsh-completion-scripts.html)
- [Reducing ZSH startup time by 95%](https://tommckenzie.dev/posts/reduce-shell-startup-time-by-lazy-loading-nvm.html)
- [How to add a progress bar to a shell script?](https://stackoverflow.com/questions/238073/how-to-add-a-progress-bar-to-a-shell-script/39898465#39898465)
- [hyperfine](https://github.com/sharkdp/hyperfine)
- [dotdrop](https://github.com/deadc0de6/dotdrop)
- [tldr pages](https://tldr.sh/)