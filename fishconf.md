# Fish configuration

This is the configuration for the fish shell. There is not much in here
since mostly it is done by functions files being directly saved to fish
functions. But environment setup is here. 

[../.config/fish/config.fish](#path "save:")

## Path

Putting the user local bin first. Bad karma in the sense I am not reusing the
path variable at the time, but it had the usr local bin at the end. Why not do
this? I am sure I will find out.

    set -x PATH /usr/local/bin /usr/bin /bin /usr/sbin /sbin
