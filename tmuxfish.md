# Fish functions

These are the functions that we define for the fish shell. Probably mainly
complex tmux startup for each project. 

## Files

Each function is saved to its own file as `name.fish`

* [../.config/fish/functions/tmuxlitprolib.fish](#litpro-lib "save:")
* [../.config/fish/functions/tmuxdotfile.fish](#dotfile "save:")
* [../.config/fish/functions/tmuxeventwhen.fish](#event-when  "save:")
* [../.config/fish/functions/tmuxopen.fish](#basic-tmux-setup  "save:")


## basic tmux setup 

Having decided to use MacVim instead of term Vim (need the scrolling and
copy/paste stuff; just not worth it), still want to use tmux for having
multiple terminals within a view. Specifically, want a main left and two
sides plus a second window for Ranger. 


    function tmuxopen 
        if tmux has-session -t $argv
        else 
            tmux new-session -s $argv -n Ranger -d
            tmux send-keys -t $argv 'ranger' C-m
            tmux new-window -n Servers -t $argv
            tmux split-window -h -t $argv:2
            tmux split-window -v -t $argv:2.2
            tmux select-window -t $argv:2
            tmux select-pane -t 1
        end
        tmux attach -t $argv
    end


## litpro lib

This is the litpro library version of tmux startup

    function tmuxlitprolib 
        if tmux has-session -t Litprolib
        else
            cd ~/repos/literateprogramming/lib
            tmux new-session -s Litprolib -n Editor -d
            tmux send-keys -t Litprolib:1 'vim -O lp.md ../full/lp.md' C-m
            tmux new-window -n Servers -t Litprolib
            tmux split-window -v -t Litprolib:2
            tmux split-window -h -t Litprolib:2.2
            tmux new-window -n Ranger -t Litprolib
            tmux send-keys -t Litprolib:3 'ranger' C-m
            tmux select-window -t Litprolib:1
            tmux select-pane -t 1
        end
        tmux attach -t Litprolib
    end 


## event when

This is the litpro library version of tmux startup

    function tmuxeventwhen 
        if tmux has-session -t Eventwhen
        else
            cd ~/repos/other/event-when
            tmux new-session -s Eventwhen -n Editor -d
            tmux send-keys -t Eventwhen:1 'vim eventwhen.md' C-m
            tmux new-window -n Servers -t Eventwhen
            tmux split-window -v -t Eventwhen:2
            tmux split-window -h -t Eventwhen:2.2
            tmux new-window -n Ranger -t Eventwhen
            tmux send-keys -t Eventwhen:3 'ranger' C-m
            tmux select-window -t Eventwhen:1
            tmux select-pane -t 1
        end
        tmux attach -t Eventwhen
    end 


## dotfile

dotfile in fish function 

    function tmuxdotfile
        if tmux has-session -t Dotfile
        else
            cd ~/dotfile
            tmux new-session -s Dotfile -n Editor -d
            tmux split-window -h -p 40 -t Dotfile
            tmux send-keys -t Dotfile:1.1 'vim tmuxfish.md' C-m
            tmux new-window -n Servers -t Dotfile
            tmux split-window -h -t Dotfile:2
            tmux split-window -v -t Dotfile:2.2
            tmux new-window -n Ranger -t Dotfile
            tmux send-keys -t Dotfile:3 'ranger' C-m
            tmux select-window -t Dotfile:1
            tmux select-pane -t 1
        end
        tmux attach -t Dotfile
    end 


