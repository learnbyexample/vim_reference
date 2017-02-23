# <a name="customizing-the-editor"></a>Customizing the editor

* [General Settings](#general-settings)
* [Text and Indent Settings](#text-and-indent-settings)
* [Mappings](#mappings)
* [Search Settings](#search-settings)
* [Custom Mappings](#custom-mappings)
* [Abbreviations](#abbreviations)
* [autocmd](#autocmd)
* [Matchit](#matchit)
* [Guioptions](#guioptions)
* [Further Reading](#further-reading)

Different programming languages require different syntax, indentation, etc. The company you work for might have its own text format and guidelines. You might want to create a short-cut for frequently used commands or create your own command

* `~/.vimrc` put your custom settings in this file, see [this sample vimrc file](https://github.com/learnbyexample/scripting_course/blob/master/.vimrc) for example
* Based on Vim version `7.4`
* Some mappings are suitable only for gvim

<br>
### <a name="general-settings"></a>General Settings

* `set history=100` increase default history from 20 to 100
* `set nobackup` don’t create backup files
    * [Vim backup files](https://stackoverflow.com/questions/607435/why-does-vim-save-files-with-a-extension)
* `set noswapfile` don’t create swap files
    * [Disabling swap files](https://stackoverflow.com/questions/821902/disabling-swap-files-creation-in-vim)
* `colorscheme murphy` a dark colored theme
* `set showcmd` show partial Normal mode command on Command Line and character/line/block-selection for Visual mode
* `set wildmode=longest,list,full` bash like Tab completion
    * first tab hit will complete as much as possible
    * second tab hit will provide a list
    * third and subsequent tabs will cycle through
* [How to set persistent Undo](https://stackoverflow.com/questions/5700389/using-vims-persistent-undo)

<br>
### <a name="text-and-indent-settings"></a>Text and Indent Settings

* `set textwidth=0` no. of characters in a line after which Vim will automatically create new line
* `filetype plugin indent on` Vim installation comes with lot of defaults
    * `:echo $VIMRUNTIME` gives your installation directory. One of them is `indent` directory which has indent styles for 100+ different file types
    * Using this setting directs Vim to apply based on file extensions like `.pl for Perl`, `.c for C`, `.v for verilog`, etc
* `set autoindent` copy indent on starting a new line
    * useful for files not affected by `filetype plugin indent on`
    * See also `:h smartindent`
* `set shiftwidth=4` defines how many characters to use when using indentation commands like `>>`
* `set tabstop=4` defines how many characters Tab key is made of
* `set expandtab` change Tab key to be made up of Space characters instead of single Tab character
* `set cursorline` highlight the cursor line
* [Show vertical bar at particular column as visual guide for required textwidth](https://superuser.com/questions/22444/make-vim-display-a-line-at-the-edge-of-the-set-textwidth)

<br>
### <a name="mappings"></a>Mappings

Mappings allow to create new commands or redefine existing ones. Mappings can be defined for specific mode. Examples given here are restricted to specific mode and non-recursive

* `nnoremap` Normal mode non-recursive mapping
* `vnoremap` Visual mode non-recursive mapping
* `inoremap` Insert mode non-recursive mapping
* `inoreabbrev` Insert mode non-recursive abbreviation
* `:h :map-commands` for more info

<br>
### <a name="search-settings"></a>Search Settings

* `set incsearch` the cursor would move to matching pattern as you type
* `set hlsearch` highlight searchpattern
* `vnoremap * y/<C-R>"<CR>` press `*` to search visually selected text in forward direction
* `vnoremap # y?<C-R>"<CR>` press `#` to search visually selected text in backward direction
* `nnoremap / /\v` automatically add very magic mode modifier for forward direction search
* `nnoremap ? ?\v` automatically add very magic mode modifier for backward direction search
* `nnoremap <silent> <Space> :noh<CR><Space>` Press Space to clear highlighted searches
    * `<silent>` modifier executes the command without displaying on Command Line
    * Note that command also retains default behavior of Space key

<br>
### <a name="custom-mappings"></a>Custom Mappings

Normal mode

* `nnoremap #2 :w<CR>` Press `F2` function key to save file. Pressing `Esc` key gets quite natural after writing text in Insert mode. Instead of multiple key presses to save using Command Line, `F2` is easier
* `nnoremap #3 :wq<CR>` Press `F3` to save the file and quit
* `nnoremap #4 ggdG` Press `F4` to delete entire contents of file
* `nnoremap #5 gg"+yG` Press `F5` to copy entire contents of file to system clipboard
* `nnoremap Y y$` change Y to behave similar to D and C
* Check out use of [Leaders](http://learnvimscriptthehardway.stevelosh.com/chapters/06.html) for even more mapping tricks

Insert mode

* `inoremap <F2> <Esc>:w<CR>a` Press `F2` to save file in Insert mode as well
    * Note the mapping sequence - it requires going to Normal mode first. Hence the `a` command to get back to Insert mode
    * `inoremap <F2> <C-o>:w<CR>` alternate way, Ctrl+o allows to execute a command and return back to insert mode automatically
* `inoremap <C-e> <Esc>ea` Ctrl+e to move to end of word
* `inoremap <C-b> <C-o>b` Ctrl+b to move to beginning of word
* `inoremap <C-a> <C-o>A` Ctrl+a to move to end of line
* `inoremap <C-s> <C-o>I` Ctrl+s to move to start of line
    * Can't use Ctrl+i remapping as it affects Tab as well
* `inoremap <C-v> <C-o>"+p` Ctrl+v to paste from clipboard in insert mode
    * Ctrl+q can be used instead of Ctrl+v to insert characters like Enter key (may not work with vim)
    * See also [How to make vim paste from (and copy to) system's clipboard?](https://stackoverflow.com/questions/11489428/how-to-make-vim-paste-from-and-copy-to-systems-clipboard)
* `inoremap <C-l> <C-x><C-l>` Ctrl+l to autocomplete matching lines
    * just saves the additional Ctrl+x, see also `:h i_CTRL-x`
* See also `:h ins-special-special`

<br>
### <a name="abbreviations"></a>Abbreviations

From typo correction to short-cuts to save typing, abbreviations are quite useful. Abbreviations get expanded only when they stand apart as a word by itself and not part of another word. For example, if the letter p is abbreviated, it is activated in variety of ways like pressing Esc, Space, Enter, Punctuations, etc not when it is part of words like pen, up, etc

* `inoreabbrev p #!/usr/bin/perl<CR>use strict;<CR>use warnings;<CR>` In Insert mode, type p followed by Enter key - this will automatically add the Perl interpreter path and two statements
* `inoreabbrev py #!/usr/bin/python3` In Insert mode, use py for Python interpreter path
* `inoreabbrev teh the` Automatically correct typo 'teh' to 'the'
* `inoreabbrev @a always @()<CR>begin<CR>end<Esc>2k$` This one works best with @a followed by Esc key in Insert mode. This inserts an empty always block (used in Verilog) and places the cursor at end of first line. After which use `i` command to type inside the parenthesis

<br>
### <a name="autocmd"></a>autocmd

Execute commands based on events

```vimL
augroup plpy
    autocmd!

    " automatically add Perl path using previously set inoreabbrev for p
    autocmd BufNewFile *.pl :normal ip
    " automatically add Python path
    autocmd BufNewFile *.py :normal ipy
    " Prevent comment character leaking to next line
    autocmd FileType * setlocal formatoptions-=r formatoptions-=o
augroup END
```

**Further Reading**

* `:h :autocmd` and `:h autocmd-groups`
* [autocmd tutorial](http://learnvimscriptthehardway.stevelosh.com/chapters/12.html)
* [augroup tutorial](http://learnvimscriptthehardway.stevelosh.com/chapters/14.html)

<br>
### <a name="matchit"></a>Matchit

* `set matchpairs+=<:>` adds <> to `%` matchpairs 

To match keywords like HTML tags, if-else pairs, etc with `%`, one can install `matchit.vim` plugin. It is not enabled by default as it is not backwards compatible

* `:echo $VIMRUNTIME` get path of installation directory
* `mkdir -p ~/.vim/plugin` create required directories from terminal or within Vim if you prefer
* `cp /usr/share/vim/vim74/macros/matchit.vim ~/.vim/plugin/` replace `/usr/share/vim/vim74` with output of echo if it is different in your case
* `:h matchit-install` for more info

<br>
### <a name="guioptions"></a>Guioptions

* `set guioptions-=m` no menu bar
* `set guioptions-=T` no tool bar
* See also `:h guioptions`

<br>
### <a name="further-reading"></a>Further Reading

* [Excellent book on Vimscript and customizing Vim](http://learnvimscriptthehardway.stevelosh.com/)
* [Vim plugins](http://vimawesome.com/)
* [Useful .vimrc tips](https://stackoverflow.com/questions/164847/what-is-in-your-vimrc)
* `~/.vimrc` from various users:
    * [vim-sensible](https://github.com/tpope/vim-sensible)
    * [sane defaults](https://github.com/nicolasmccurdy/sane-defaults/blob/master/home/.vimrc)
    * [minimal for new users](https://gist.github.com/benmccormick/4e4bc44d8135cfc43fc3)
    * [generate vimrc](http://vimconfig.com/)
    * [building vimrc from scratch](https://marcgg.com/blog/2016/03/01/vimrc-example)
* [adding custom keywords to match on %](https://stackoverflow.com/questions/27498221/vim-highlight-matching-begin-end)
* [using gf to open file in new tab or splits](https://vi.stackexchange.com/questions/3364/open-filename-under-cursor-like-gf-but-in-a-new-tab-or-split)
* [open file under cursor with gf based on current file extension](https://stackoverflow.com/questions/33093491/vim-gf-with-file-extension-based-on-current-filetype)
* [Vim history](https://stackoverflow.com/questions/10752863/vim-record-history)
* [Indenting all the files in folder](https://stackoverflow.com/questions/3218528/indenting-in-vim-with-all-the-files-in-folder)
