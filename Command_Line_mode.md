# <a name="command-line-mode"></a>Command Line Mode

* [Saving changes](#saving-changes)
* [Exit Vim](#exit-vim)
* [Combining Save and Quit](#combining-save-and-quit)
* [Editing buffers](#editing-buffers)
* [Search](#search)
* [Search and Replace](#search-and-replace)
* [Editing lines filtered by pattern](#editing-lines-filtered-by-pattern)
* [Shell Commands](#shell-commands)
* [Miscellaneous](#miscellaneous)

Any operation is completed by pressing `Enter` key after which the mode changes back to Normal mode  
Press `Esc` key to ignore whatever is typed and return to Normal mode

<br>
### <a name="saving-changes"></a>Saving changes

* `:w` save changes
* `:w filename` provide a filename if it is a new file or if you want to save to another file
* `:w!` save changes even if file is read-only, provided user has appropriate permissions
* `:wa` save changes made to all the files opened

<br>
### <a name="exit-vim"></a>Exit Vim

* `:q` quit the current file (if other tabs are open, they will remain) - if unsaved changes are there, you will get an error message
* `:qa` quit all
* `:q!` quit and ignore unsaved changes

<br>
### <a name="combining-save-and-quit"></a>Combining Save and Quit

* `:wq` save changes and quit
* `:wq!` save changes even if file is read-only and quit

<br>
### <a name="editing-buffers"></a>Editing buffers

Multiple files can be opened in Vim within same tab and/or different tab

**Buffers**

* `:e` refreshes the current buffer
* `:e filename` open file for editing
* `:e#` switch back to previous buffer
    * `Ctrl+6` can also be used
* `:e#1` open first buffer
* `:e#2` open second buffer and so on
* `:bn` open buffer next
* `:bp` open buffer previous

**Splitting**

* `:split filename` open file for editing in new horizontal split screen, Vim adds a highlighted horizontal bar with filename for each file thus opened
    * use `:sp filename` for short
* `:vsplit filename` open file for editing in new vertical split screen
    * use `:vs filename` for short

**Multiple tabs**

* `:tabe filename` open file for editing in new tab instead of adding to buffers
    * `:h :tabe` for more info and different options
* `:tabn` to go to next tab
    * `gt` and `Ctrl+Page Down` can also be used
    * `2gt` to move to second tab
* `:tabp` to go to previous tab
    * `gT` and `Ctrl+Page Up`  can also be used
* `:tabr` to go to first tab (r for rewind)
* `:tabl` to go to last tab (l for last)
* `:tabm 0` move current tab to beginning
* `:tabm 2` move current tab to 3rd position from left
* `:tabm` move current tab to end

**Making changes to all buffers**

If multiple buffers are open and you want to apply common editing across all of them, use `bufdo` command

* `:silent! bufdo %s/searchpattern/replacestring/g | update` substitute across all buffers, `silent` skips displaying normal messages and `!` skips error messages as well
* It is not an efficient way to open buffers just to search and replace a pattern across multiple files, use tools like `sed,awk,perl` instead
* `:h :bufdo`, `:h :windo` and `:h :silent` for more info
* [how to change multiple files](https://vimcasts.org/episodes/using-argdo-to-change-multiple-files/)
* [Effectively work with multiple files](https://stackoverflow.com/questions/53664/how-to-effectively-work-with-multiple-files-in-vim)
* [When to use Buffers and when to use Tabs](https://joshldavis.com/2014/04/05/vim-tab-madness-buffers-vs-tabs/)

<br>
### <a name="search"></a>Search

* `/searchpattern` search the given pattern in forward direction
    * Use `n` command to move to next match and `N` for previous match
* `?searchpattern` search the given pattern in backward direction
    * `n` command goes to next match in backward direction and `N` moves to next match in forward direction
* Press `Esc` key to ignore typed pattern search and return to Normal mode

By default, cursor is placed at starting character of match

* `/searchpattern/s+2` places cursor 2 characters after start of match
* `/searchpattern/s-2` places cursor 2 characters before start of match
* `/searchpattern/e` places cursor at end of match
* `/searchpattern/e+4` places cursor 4 characters after end of match
* `/searchpattern/e-4` places cursor 4 characters before end of match
* `/searchpattern/+3` places cursor 3 lines below match
* `/searchpattern/-3` places cursor 3 line above match

Highlight settings

* `:set hlsearch` highlights the matched pattern
* `:set nohlsearch` do not highlight matched pattern
* `:set hlsearch!` toggle highlight setting
* `:noh` clear highlighted patterns, doesn't affect highlight settings

<br>
### <a name="search-and-replace"></a>Search and Replace

General syntax is `:range s/searchpattern/replacestring/flags`  
`s` is short-form for `substitute`

* `:. s/a/b/` replace first occurrence of character a with character b on current line only
* `:2 s/apple/Mango/i` replace first occurrence of word apple with word Mango only on second line
    * `i` flag ignores case sensitivity for searchpattern
* `:3,6 s/call/jump/g` replace all occurrences of call to jump on lines 3 to 6
    * `g` flag changes default behavior to search and replace all occurrences
* `:1,$ s/call/jump/g` replace all occurrences of call to jump in entire file
    * `$` points to last line of file
* `:% s/call/jump/g` replace all occurrences of call to jump in entire file
    * `%` is short-cut for the range `1,$`
* More on substitute and Regular Expressions is covered in a later chapter. `:h :substitute` and `:h range` for more info

<br>
### <a name="editing-lines-filtered-by-pattern"></a>Editing lines filtered by pattern

The `g` command, short for `global` allows to edit lines that are first filtered based on a searchpattern

* `:g/call/d` delete all lines containing the word call
* `:1,5 g/call/d` delete lines containing the word call only from first five lines
* `:v/jump/d` delete all lines NOT containing the word jump
* `:g/cat/ s/animal/mammal/g` replace 'animal' with 'mammal' only on lines containing the searchpattern 'cat'
* `:.,+20 g/^#/ normal >>` indent from current line to next 20 lines that start with # character

For more info, see `:h :g`
Also, check out `:h :t` and `:h :m` to copy/move the filtered lines using `:g` and `:h ex-cmd-index` for complete list of commands to use with `:g`

<br>
### <a name="shell-commands"></a>Shell Commands

One can also use shell commands within Vim

* `:.! date` replace current line with output of `date` command
* `:%! sort` sort all lines of file
* `:3,8! sort` sort only lines 3 to 8 of file
* `:r! date` insert output of `date` command below current line
* `:r report.log` insert contents of file report.log below current line
    * Note that `!` is not used as we are reading a file, not using external shell command
* `:.!grep '^Help ' %` replace current line with all line starting with Help in current file
* `:sh` open a shell session within Vim, use `exit` command to return to editing
* [Working with external commands](https://www.linux.com/learn/tutorials/442419-vim-tips-working-with-external-commands)
* `:h :!`, `:h :sh` and `:h :r` for more info

<br>
### <a name="miscellaneous"></a>Miscellaneous

* `:set number` prefix line numbers (it is a visual guideline, won't modify text)
* `:set nonumber` remove line number prefix
* `:set number!` toggle number setting
* `:set relativenumber` prefix line numbers relative to current line
    * current line is 0, 1 for lines above and below, 2 for two lines above and below and so on
    * useful visual guide for commands like `11yy, 6>>, 9j` etc
* `:set norelativenumber` remove relative line number prefix
* `:set relativenumber!` toggle relative number setting
