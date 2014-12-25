# Tmux Configuration

This is a literate program that generates the tmux configuration file that I use. It expects to be in one directory below where the final tmux conf file should be. 

[../.tmux.conf](#tmux-conf "save:")

## Tmux Conf

This is the outline of the file

    _"prefix"

    _"shell"

    _"re source"

    _"count"

    _"x"

_"terminal"

    _"pipes"

    _"send prefix"

    _"movement"

    _"mouse"

    _"copy"

### Prefix

Change the prefix in tmux from control B to control A, freeing up control B

    set -g prefix C-a
    unbind C-b

### Shell

We set the default shell to fish

    set-option -g default-shell /usr/local/bin/fish 


## Re source

We want to make changes on the fly to the conf file and have restarted. This line does this by binding prefix r  to reload the file .tmux.conf

    bind r source-file ~/.tmux.conf \; display "Reloaded!"

## Count

This sets the counting to start at 1. That's so that we can refer to the numbers starting wth 1 rather than 0 and then 1. 

    set -g base-index 1
    setw -g pane-base-index 1

## x

    unbind x
    bind x kill-pane
    bind X kill-session

## terminal

    set -g default-terminal “screen-256color”

## pipes

This will split windows and create them in current directory. Also added new window to do the same. This comes from [SO](http://unix.stackexchange.com/questions/101949/new-tmux-panes-go-to-the-same-directory-as-the-current-pane-new-tmux-windows-go)

    bind | split-window -h -c "#{pane_current_path}"
    bind - split-window -v -c "#{pane_current_path}"
    bind c new-window -c "#{pane_current_path}"

## send prefix
    
    bind C-a send-prefix

##  movement

This remaps the vim movement keys under prefix to switch windows

    bind h select-pane -L 
    bind j select-pane -D 
    bind k select-pane -U 
    bind l select-pane -R

These will switch between windows (not panes).

    bind -r C-h select-window -t :- 
    bind -r C-l select-window -t :+

And to resize the panes

    bind -r H resize-pane -L 5 
    bind -r J resize-pane -D 5 
    bind -r K resize-pane -U 5 
    bind -r L resize-pane -R 5


## Mouse

Turn off mouse

    setw -g mode-mouse off

## copy

This is just to enable the vi keys, get paste board copy working, bind vi
paste keys, and map the visual copy keys.

    setw -g mode-keys vi


    unbind [
    bind Escape copy-mode
    unbind p
    bind p paste-buffer
    bind -t vi-copy 'v' begin-selection 
    bind -t vi-copy 'y' copy-selection
    
    
    set -g default-command "reattach-to-user-namespace -l /usr/local/bin/fish"
    
    bind C-c run "tmux save-buffer - | reattach-to-user-namespace pbcopy"
    bind C-v run "tmux set-buffer (reattach-to-user-namespace pbpaste); tmux paste-buffer"

[]()

This is from Tmux guide from pragmmatic programmers

To use this wrapper script, we first clone the repository and
compile the wrapper.

    $ git clone https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard.git
    $ cd tmux-MacOSX-pasteboard/
    $ make reattach-to-user-namespace

Then, we move the file to someplace on our PATH, like `/usr/local/bin:`

    $ sudo mv reattach-to-user-namespace /usr/local/bin


## Links

*  A bit about changing default shell and re sourcing: [super-user](http://superuser.com/questions/253786/how-can-i-make-tmux-use-my-default-shell)
