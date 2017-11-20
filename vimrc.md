# Vimrc

This is my literate-programming file for generating my vimrc file. 

    _"shell"

    _"init required"

    _"vundle"

    _"plugin followups"

    _"rest"

    _"vim 15"

[../.vimrc](# "save:")


## Vundle

This is the list of plugins, managed by vundle
    


Vundle proper

    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/vundle/
    call vundle#begin()
    " " alternatively, pass a path where Vundle should install bundles
    " "let path = '~/some/path/here'
    " "call vundle#rc(path)
    "
    " " let Vundle manage Vundle, required
    Bundle 'gmarik/vundle'

    " " The following are examples of different formats supported.
    " " Keep plugin commands between here and filetype plugin indent on.
    " " scripts on GitHub repos
    Plugin 'tpope/vim-fugitive'
    Plugin 'vim-voom/VOoM'
    Plugin 'tpope/vim-markdown'
    Plugin 'Shutnik/jshint2.vim'

Vim vinegar helps with navigating file structures. To fix a bug about the
dash, modify the plugin with the vingear below. 



    Plugin 'tpope/vim-vinegar'

Repeat allows plugin commands to be repeated

    Plugin 'tpope/vim-repeat'

Surround allows one to surround something parentheses or tags and remove them:
`cs'"` or `ds"` or `ysiw<em>`
    
    Plugin 'tpope/vim-surround'

And for Ack stuff for searching [Ack](https://vimawesome.com/plugin/ack-vim)

    Plugin 'mileszs/ack.vim'

And this enables stuff for [fzf](https://github.com/junegunn/fzf.vim)

    Plugin 'junegunn/fzf.vim'

The rest of Vundle closing 

    call vundle#end()
    filetype plugin indent on    " required
    " To ignore plugin indent changes, instead use:
    " "filetype plugin on
    " "
    " " Brief help
    " " :PluginList       - lists configured plugins
    " " :PluginInstall    - installs plugins; append `!` to update or just
    " :PluginUpdate
    " " :PluginSearch foo - searches for foo; append `!` to refresh local cache
    " " :PluginClean      - confirms removal of unused plugins; append `!` to
    " auto-approve removal
    " "
    " " see :h vundle for more details or wiki for FAQ
    " " Put your non-Plugin stuff after this line

## plugin followups

This is stuff that follows up on the plugins. 

    "jshint
    let jshint2_read = 1
    let jshint2_save = 1

    " Markdown
    let g:markdown_fenced_languages = ['css',  'javascript', 'js=javascript', 'json=javascript', 'xml', 'html']
    au BufRead,BufNewFile *.md set filetype=markdown
    " keep markdown lines short
    autocmd FileType markdown setlocal textwidth=78




## shell

This deals with the shell being fish instead bash. 

    " https://github.com/gmarik/Vundle.vim/issues/175
    if $SHELL =~ 'bin/fish'
        set shell=/bin/sh
    endif

## Init required

This seems to be just what one always has to be Vim

    set nocompatible              " be iMproved, required
    filetype off                  " required

## rest

Need to split all 

Change the leader to a comma. Can also apply a bunch of leaders with one
leader, then assign it and do with the other. Also showcmd will show that you
ahve typed it. 

    set runtimepath+=/usr/local/opt/fzf
    let mapleader = ","

    set tabstop=4
    set shiftwidth=4
    set expandtab
    set autoindent


    " When started as "evim", evim.vim will already have done these settings.
    if v:progname =~? "evim"
      finish
    endif

    " allow backspacing over everything in insert mode
    set backspace=indent,eol,start

    set nobackup		" keep a backup file
    set history=50		" keep 50 lines of command line history
    set ruler		" show the cursor position all the time
    set showcmd		" display incomplete commands
    set incsearch		" do incremental searching

    " For Win32 GUI: remove 't' flag from 'guioptions': no tearoff menu entries
    " let &guioptions = substitute(&guioptions, "t", "", "g")

    " Don't use Ex mode, use Q for formatting
    map Q gq

    " CTRL-U in insert mode deletes a lot.  Use CTRL-G u to first break undo,
    " so that you can undo CTRL-U after inserting a line break.
    inoremap <C-U> <C-G>u<C-U>

    " In many terminal emulators the mouse works just fine, thus enable it.
    if has('mouse')
      set mouse=a
    endif

    " Switch syntax highlighting on, when the terminal has colors
    " Also switch on highlighting the last used search pattern.
    if &t_Co > 2 || has("gui_running")
      syntax on
      set hlsearch
    endif

    " Only do this part when compiled with support for autocommands.
    if has("autocmd")

      " Enable file type detection.
      " Use the default filetype settings, so that mail gets 'tw' set to 72,
      " 'cindent' is on in C files, etc.
      " Also load indent files, to automatically do language-dependent indenting.
      filetype plugin indent on

      " Put these in an autocmd group, so that we can delete them easily.
      augroup vimrcEx
      au!

      " For all text files set 'textwidth' to 78 characters.
      autocmd FileType text setlocal textwidth=78

      " When editing a file, always jump to the last known cursor position.
      " Don't do it when the position is invalid or when inside an event handler
      " (happens when dropping a file on gvim).
      " Also don't do it when the mark is in the first line, that is the default
      " position when opening a file.
      autocmd BufReadPost *
        \ if line("'\"") > 1 && line("'\"") <= line("$") |
        \   exe "normal! g`\"" |
        \ endif

      augroup END

    else

      set autoindent		" always set autoindenting on

    endif " has("autocmd")

    " Convenient command to see the difference between the current buffer and the
    " file it was loaded from, thus the changes you made.
    " Only define it when not defined already.
    if !exists(":DiffOrig")
      command DiffOrig vert new | set bt=nofile | r # | 0d_ | diffthis
              \ | wincmd p | diffthis
    endif

    "search for selected text, forwards or backwards.
    vnoremap <silent> * :<C-U>
      \let old_reg=getreg('"')<Bar>let old_regtype=getregtype('"')<CR>
      \gvy/<C-R><C-R>=substitute(
      \escape(@", '/\.*$^~['), '\_s\+', '\\_s\\+', 'g')<CR><CR>
      \gV:call setreg('"', old_reg, old_regtype)<CR>
    vnoremap <silent> # :<C-U>
      \let old_reg=getreg('"')<Bar>let old_regtype=getregtype('"')<CR>
      \gvy?<C-R><C-R>=substitute(
      \escape(@", '?\.*$^~['), '\_s\+', '\\_s\\+', 'g')<CR><CR>
      \gV:call setreg('"', old_reg, old_regtype)<CR>


    "hi User1 guifg=#eea040 guibg=#222222
    "hi User2 guifg=#dd3333 guibg=#222222
    "hi User3 guifg=#ff66ff guibg=#222222
    "hi User4 guifg=#a0ee40 guibg=#222222
    "hi User5 guifg=#eeee40 guibg=#222222

    set guifont=Source\ Code\ Pro:h13

    " path
    set statusline=%F
    " flags
    set statusline+=%m%r%h%w
    " encoding
    set statusline+=\ %{strlen(&fenc)?&fenc:&enc}
    "date time
    set statusline+=\ %=%{strftime('%a\ %I:%M\ %p')}
    " line x of y
    set statusline+=\ %l\/%L
    set statusline+=\ %4v
    set laststatus=2

    set linebreak
    set showbreak=>\ 

    set dir=~/.vim/swp  "don't pollute directory with swap
    set ww=<,>,[,] "allow left right cursors to wrap

    autocmd InsertEnter * set cul 
    autocmd InsertLeave * set nocul
    au FocusLost,TabLeave * call feedkeys("\<C-\>\<C-n>")

    "Splits new window right or bottom depending on split
    set splitright
    set splitbelow

    "create shell command to generate new window to see output
    command! -complete=shellcmd -nargs=+ Shell call s:RunShellCommand(<q-args>)
    function! s:RunShellCommand(cmdline)
      echo a:cmdline
      let expanded_cmdline = a:cmdline
      for part in split(a:cmdline, ' ')
         if part[0] =~ '\v[%#<]'
            let expanded_part = fnameescape(expand(part))
            let expanded_cmdline = substitute(expanded_cmdline, part, expanded_part, '')
         endif
      endfor
      belowright vnew
      setlocal buftype=nofile bufhidden=wipe nobuflisted noswapfile nowrap
      call setline(1, 'You entered:    ' . a:cmdline)
      call setline(2, 'Expanded Form:  ' .expanded_cmdline)
      call setline(3,substitute(getline(2),'.','=','g'))
      execute '$read !'. expanded_cmdline
      setlocal nomodifiable
      1
    endfunction

    "defines Npm shell command to run. useful with Npm test
    command! -complete=file -nargs=* Npm call s:RunShellCommand('npm '.<q-args>)


    "backspace acts same in normal
    nnoremap <silent> <BS> i<BS><Esc> 
    "enter inserts a new line
    "nnoremap <silent> <Enter> o<Esc> 
    "shift enter inserts a new line above
    "nnoremap <silent> <S-Enter> ko<Esc>k 
    "map tab to switch windows
    nnoremap <silent> <Tab> <C-w><C-w>
    "map yu to formatting current paragraph
    "nnoremap yu gqap 
    " ctrl tab closes window
    nnoremap <silent> <C-Tab> <C-w>c
    " grabbing last pasted stuff
    nnoremap gp `[v`]

    " Shortcut to rapidly toggle `set list`
    nmap <leader>l :set list!<CR>
     
    " Use the same symbols as TextMate for tabstops and EOLs
    set listchars=tab:▸\ ,eol:¬

    "Invisible character colors
    highlight NonText guifg=#cccccc
    highlight SpecialKey guifg=#cccccc



    " from http://vim.wikia.com/wiki/Set_working_directory_to_the_current_file 

    nnoremap ,cd :cd %:p:h<CR>:pwd<CR>

    " from http://stackoverflow.com/questions/657447/vim-clear-last-search-highlighting 
    " This unsets the last search pattern register by hitting return
    nnoremap <CR> :noh<CR><CR>

    "http://stackoverflow.com/questions/2287440/how-to-do-case-insensitive-search-in-vim
    :set ignorecase


Toggle on [spell checking](http://danawoodman.com/posts/spellcheck-markdown-vim/) in markdown files. 

    " Turn spellcheck on for markdown files.
    autocmd BufNewFile,BufRead *.md set spell

Make permanent the [quick time escape](http://vim.wikia.com/wiki/To_switch_back_to_normal_mode_automatically_after_inaction)

Founding it annoying since if I think I am editing, than the normal mode
commands get whacky. 

    " automatically leave insert mode after 'updatetime' milliseconds of inaction
    " au CursorHoldI * stopinsert

## Vim 15 

Using [VIM after 15 years](https://statico.github.io/vim3.html)

install fzf using brew  `brew install fzf`

    nmap ; :Buffers<CR>
    nmap <Leader>f :Files<CR>
    nmap <Leader>t :Tags<CR>

Install ag using `brew install the_silver_searcher`  [silver searcher](https://github.com/ggreer/the_silver_searcher)

    let g:ackprg = 'ag --nogroup --nocolor --column'    


## Vinegar

To fix a bug, do the following: 

replace 

    let s:netrw_up = substitute(maparg('-', 'n'), '\c^:\%(<c-u>\)\=', '', '') 

with 

    if strpart(g:loaded_netrw, 1, strlen(g:loaded_netrw)) < 162 
        let s:netrw_up = substitute(maparg('-', 'n'), '\c^:\%(<c-u>\)\=', '', '') 
    else 
        let s:netrw_up = substitute(maparg('<Plug>NetrwBrowseUpDir', 'n'), '\c^:\%(<c-u>\)\=', '', '') 
    endif
    
