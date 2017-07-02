# Fish configuration

This is the configuration for the fish shell. There is not much in here
since mostly it is done by functions files being directly saved to fish
functions. But environment setup is here. 

[../.config/fish/config.fish](#path "save:")

Just a nice tidbit:  https://github.com/edc/bass

## Path

Putting the user local bin first. Bad karma in the sense I am not reusing the
path variable at the time, but it had the usr local bin at the end. Why not do
this? I am sure I will find out.

    set -x PATH /usr/local/bin /usr/bin /bin /usr/sbin /sbin /Users/jostylr/npm/bin



## Vim function startup

This starts up vim with a default 

    function project
        switch $argv[1] 
        _":lp|sub NAME, lib, project, lp" 
        _":lp|sub NAME, full, project, lp" 
        _":lp|sub NAME, cli, project, lp" 
        _":lp|sub NAME, litpro" 
        _":lp|sub NAME, testing" 
        case eventwhen  
           pushd ~/repos/other/event-when
           mvim eventwhen.md -c "set fu" -c "vnew" -c "vertical resize 80" -c "split test/test.md"
           popd
        case mwia  
           pushd ~/repos/writeweb/mwia
           git pull
           mvim -c "Ex" -c "set fu" -c "vnew" -c "vertical resize 80" -c "Ex"
           popd
        case fish 
           pushd ~/dotfile
           mvim fishconf.md -c "set fu" -c "vnew" -c "vertical resize 10" 
           popd
        case vimrc 
           pushd ~/dotfile
           mvim vimrc.md -c "set fu" -c "vnew" -c "vertical resize 10" 
           popd
        case ai-web  
           pushd ~/repos/ai/website-pages
           mvim project.md -c "set fu" -c "vnew" -c "vertical resize 80"
           popd
       case server
           pushd ~/repos/ai/server
           mvim project.md -c "set fu" -c "vnew" -c "vertical resize 80"
           popd
        end 
    end


[../.config/fish/functions/project.fish](# "save:")


[lp]()

This is the literate programming stub starts. 

    case NAME 
       pushd ~/repos/literateprogramming/NAME
       mvim project.md -c "set fu" -c "vnew" -c "vertical resize 80" -c "Sex tests"
       popd

[lpp]() 

plugin

    _":lp|sub /NAME, /plugins/NAME" 

## mynvm

This creates an alias for nvm and its use in [bass](https://github.com/edc/bass) : 


    function mynvm
       bass source ~/.nvm/nvm.sh ';' nvm $argv
    end

This restores node to the system. The idea is that globally installed "apps"
using node should have your system node, most likely. nvm is used largely for
checking compatibility with different versions that one wants to support. So
use that to switch, do the tests, and then switch back to the node app setup. 

    function appnode
       bass source ~/.nvm/nvm.sh ';' nvm use system
    end

[../.config/fish/functions/mynvm.fish](# "save:")

And on the subject of nvm, installing gave me this message:

```ignore
=> nvm is already installed in /Users/jostylr/.nvm, trying to update using git
=> * (detached from v0.25.4)
  master

=> Appending source string to /Users/jostylr/.bash_profile
=> You currently have modules installed globally with `npm`. These will no
=> longer be linked to the active version of Node when you install a new node
=> with `nvm`; and they may (depending on how you construct your `$PATH`)
=> override the binaries of modules installed with `nvm`:

/Users/jostylr/npm/lib
├── iron-node@1.2.11
├── literate-programming@0.8.4
├── litpro@0.10.0
├── nodemon@1.3.7
└── npm-check@3.2.10

=> If you wish to uninstall them at a later point (or re-install them under your
=> `nvm` Nodes), you can remove them from the system Node as follows:

     $ nvm use system
     $ npm uninstall -g a_module

=> Close and reopen your terminal to start using nvm
```
