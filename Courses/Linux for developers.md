## Dotfiles

### Repo's examples
- https://github.com/Tuurlijk/dotfiles
- https://github.com/denisidoro/dotfiles
- https://github.com/rgomezcasas/dotfiles

### Folder structure
- bin/
	- dot
	- git-co
	- git-discard
	- git-undo
- doc/
	- navi
- editors/
	- intellij
	- vs-code
- git/
	- .gitconfig
	- gitignore_global
- langs/
	- cloujre
	- js
		- global_modules.txt
- shell/
	- zsh
	- bash
	- \_aliases
	- \_exports
- so/
	- linux
- simlinks/
	- links.sh

## Tools for everyday
- https://github.com/rupa/z
- https://github.com/tmux/tmux
- https://github.com/jarun/nnn
- https://github.com/junegunn/fzf
- https://github.com/denisidoro/navi

## Package manager

### Node version management
- https://volta.sh/
- https://github.com/nvm-sh/nvm
- https://github.com/tj/n

### Software Development Kit Manager
- https://sdkman.io/

## See your frequently commands
```
fc -lim "*$@*" 1 | awk '{print $4}' | sort | uniq -c | sort -rn | head
```


## Alias

- [[Alias]]

## Launchers

### Rofi
- https://davatorium.github.io/rofi/

```
ls ~/Projects | rofi -show -dmenu -font "Monospace 30" -i | xargs -I_ idea ~/Projects/_
```

### Albert
- https://albertlauncher.github.io/

### dmenu
```
ls ~/Projects | dmenu | xargs -I_ idea ~/Projects/_
```

### fzf
```
ls ~/Projects | fzf | xargs -I_ idea ~/Projects/_
```

### Emoji launcher
```
Meta + .
```

## Trackpad

- https://kaigo.medium.com/mac-like-gestures-on-ubuntu-20-04-dell-xps-15-7ea6e3be7f76
- https://github.com/bulletmark/libinput-gestures.git
- https://gitlab.com/cunidev/gestures

## n³ The unorthodox terminal file manager

- https://github.com/jarun/nnn
	- https://github.com/jarun/nnn/wiki/Advanced-use-cases#file-icons
	- https://github.com/jarun/nnn/tree/master/plugins

### Setup
```
BLK="04" CHR="04" DIR="04" EXE="00" REG="00" HARDLINK="00" SYMLINK="06" MISSING="00" ORPHAN="01" FIFO="0F" SOCK="0F" OTHER="02"
export NNN_FCOLORS="$BLK$CHR$DIR$EXE$REG$HARDLINK$SYMLINK$MISSING$ORPHAN$FIFO$SOCK$OTHER"
export NNN_PLUG='j:z;f:fzcd;p:preview-tui-ext'
export NNN_FIFO=/tmp/nnn.fifo
```

## Screenshot
- https://github.com/flameshot-org/flameshot