---
layout: post
title: Using Tmux More
---

###Useful Tmux Commands I have Compiled

`tmux new -s session_name`
creates a new tmux session named session_name

`tmux attach -t session_name`
attaches to an existing tmux session named session_name

`tmux switch -t session_name`
switches to an existing session named session_name

`tmux list-sessions`
lists existing tmux sessions

`tmux detach (prefix + d)`
detach the currently attached session

`tmux new-window (prefix + c)`
create a new window

`tmux select-window -t :0-9 (prefix + 0-9)`
move to the window based on index

`tmux rename-window (prefix + ,)`
rename the current window

`Prefix + z`
Zoom into currently focused pane


Add this to the remote ~/.bash_profile
`if [ -z "$TMUX" ]; then
    tmux attach -t default || tmux new -s default
fi`


###My .tmux.conf
link to the github version of this file https://github.com/devinbfergy/linux_configs/blob/master/.tmux.conf
```# remaping the prefix to screens version
set -g prefix C-a
bind C-a send-prefix

# Quality of life stuff
set -g history-limit 10000
set -g allow-rename off

## Join Windows
bind-key j command-prompt -p "join pange from:"  "join-pain -s '%%'"
bind-key s command-prompt -p "send pane to:"  "join-paine -t '%%'"

#Search mode VI
set-window-option -g mode-keys vi

run-shell /opt/tmux-logging/logging.tmux

#allows for the config file to be reloaded
bind r source-file ~/.tmux.conf \; display "Reloaded config"

#vertical split
bind V split-window -h

#horizontal axis split
bind H split-window
```

