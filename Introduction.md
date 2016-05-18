# <a name="introduction"></a>Introduction

* [Ice Breaker](#ice-breaker)
* [Vim help and resources](#vim-help-and-resources)
* [Opening Vim](#opening-vim)
* [Modes of Operation](#modes-of-operation)
* [Insert mode](#insert-mode)
* [Normal mode](#normal-mode)
* [Visual mode](#visual-mode)
* [Command Line mode](#command-line-mode)
* [Identifying current mode in gvim](#identifying-current-mode-in-gvim)

Vim is a text editor, powerful one at that. It has evolved from vi to vim (Vi Improved) and gvim (Vim with GUI)

* Vim is not a full fledged writing software like LibreOffice Writer or MS Word. It is a text editor, mainly used for writing programs
* Notepad in Windows is an example for text editor. There are no formatting options like bold, heading, page numbers etc
* If you have written C program within IDE, you might have noticed that keywords are colored (syntax highlighting), automatic code indentation, etc. Vim provides all of that and more

Vim has programmable abilities on editing tasks. One can also execute shell commands right within Vim and incorporate their outputs. Text can be modified, sorted, regular expressions be applied for complicated search and replace. Record a series of editing commands and execute on other portions of text in same file or another file. To sum it up - for most editing tasks, no need to write a program, they can be managed within Vim

Now, one might wonder, what is all this need for complicated editing features? Why does a text editor require programming capablities? Why is there even a requirement to learn using a text editor? All one needs from editor is writing text with keyboard, use Backspace or Delete key, have some GUI button for opening and saving, preferrably a search and replace feature, etc.

A simple and short answer is: to reduce repititive manual task and the time taken to type a computer code. Faster editing probably makes sense. Reduce typing time? seems absurd. But when you are in the middle of coding a large program, thinking about complicated logic, it helps to have minimal typing distraction

<br>
### <a name="ice-breaker"></a>Ice Breaker

Open a terminal and try these steps:

* `gvim first_vim.txt` opens the file first_vim.txt for editing (use `vim` if `gvim` is not available)
* Press `i` key (yes, the lowercase alphabet i, not any special key)
* Start typing, say 'Hello Vim'
* Press `Esc` key
* Press `:` key
* Type `wq`
* Press `Enter` key
* `cat first_vim.txt` sanity check to see what you typed is saved or not

Phew, what a complicated procedure to write a simple line of text, isn't it? This is the most challenging and confusing part for a newbie to Vim. Don't proceed any further if you haven't got a hang of this, take help of [youtube videos](https://www.youtube.com/results?search_query=vim+editor) if you must. Master this basic procedure and you are ready for awesomeness of Vim

* Material presented here is based on `gvim`, which has a few subtle differences from `vim` - see this [stackoverflow](https://stackoverflow.com/questions/22517896/linux-gvim-vs-vim) thread for details

<br>
### <a name="vim-help-and-resources"></a>Vim help and resources

* `gvimtutor` shell command. Opens a tutor file and has various lessons to get started with Vim
    * `vimtutor` if gvim is not available
* For built-in help, use `Help` menu in gvim, or type `:help` in Normal mode (`:h` is short for `:help`)
    * `:h usr_toc.txt` table of contents for Vim User Manual - 'Task oriented explanations, from simple to complex.  Reads from start to end like a book.'
    * `:h reference_toc` table of contents for Reference Manual - 'Precise description of how everything in Vim works.'
* [best introduction to Vi and its core editing concepts explained as a language](https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118)
    * this stackoverflow thread also has numerous tips and tricks to use Vim
* [Vim curated resources](https://github.com/learnbyexample/scripting_course/blob/master/Vim_curated_resources.md)

<br>
### <a name="opening-vim"></a>Opening Vim

* `gvim` new document when filename is not specified
* `gvim script.sh` opens script.sh, blank document if script.sh doesn't exist
* `gvim report.log power.log area.log` open specified files, but only report.log is in the current buffer. Use `:n` (short for `:next`) to go to next buffer for editing
* `gvim -p report.log power.log area.log` open specified files in separate tabs
* `gvim -o report.log power.log area.log` open specified files as horizontal splits
* `gvim -O report.log power.log area.log` open specified files as vertical splits
* `gvim -R script.sh` opens script.sh in Readonly mode, changes can still be made and saved despite warning messages shown (for example, by using `:w!`)
* `gvim -M script.sh` opens script.sh in stricter Readonly mode, Vim won't allow any changes to be made unless `:set modifiable` is used and file can't be saved until `:set write` is used
* `gvim +/while script.sh` open script.sh and place the cursor under first occurrence of while
* `gvim -h` for complete list of options

<br>
### <a name="modes-of-operation"></a>Modes of Operation

Vim is best described as a modal editor. Here, we'll mainly see four modes

* Insert mode
* Normal mode
* Visual mode
* Command Line mode

For complete list of modes, see `:h vim-modes-intro` and `:h mode-switching`

<br>
### <a name="insert-mode"></a>Insert mode

This is the mode where required text is typed. Usual editing options available are Delete, Backspace and Arrow keys. Some smart editing short-cuts:

* `Ctrl+w` delete the current word
* `Ctrl+p` auto complete word based on matching words in backward direction, if more than one word matches, they are displayed as drop-down list
* `Ctrl+n` auto complete word based on matching words in forward direction
* `Ctrl+x Ctrl+l` auto complete line, if more than one line matches, they are displayed as drop-down list
* `Ctrl+t` indent current line
* `Ctrl+d` unindent current line

Pressing `Esc` key changes the mode back to Normal mode

<br>
### <a name="normal-mode"></a>Normal mode

This is the default mode when Vim is opened. This mode is used to run commands for editing operations like copy, delete, paste, recording, moving around file, etc. It is also referred to as Command mode

Below commands are commonly used to change modes from Normal mode:

**Changing to Insert mode**

* `i` insert, pressing i (lowercase) key changes the mode to Insert mode, places cursor to the left
* `a` append, pressing a (lowercase) key changes the mode to Insert mode, places cursor to the right
* `I` to place the cursor left of first non-blank character of line (helpful for indented lines)
* `gI` to place the cursor at first column of line
* `gi` to place the cursor at same position where it was left last time in Insert mode
* `A` to place the cursor at end of line
* `o` open new line below the current line and change to Insert mode
* `O` to open new line above the current line and change to Insert mode
* `s` delete character under the cursor and change to Insert mode
* `S` deletes entire line and change to Insert mode

**Changing to Command Line mode**

* `:` changes the mode to Command Line mode, awaiting further commands
* `/` change to Command Line mode for searching in forward direction
* `?` to start searching in backward direction

<br>
### <a name="visual-mode"></a>Visual mode

Visual mode is used to edit text by selecting them first  
Selection can either be done using mouse or using visual commands

* `v` start visual selection
* `V` visually select current line
* `Ctrl+v` visually select column(s)

Pressing `Esc` key changes the mode back to Normal mode

<br>
### <a name="command-line-mode"></a>Command Line mode

* After `:` or `/` or `?` is pressed in Normal mode, the prompt appears in last line of Vim window
* This mode is used to perform file operations like save, quit, search, replace, shell commands, etc.
* Any operation is completed by pressing `Enter` key after which the mode changes back to Normal mode

Press `Esc` key to ignore whatever is typed and return to Normal mode

<br>
### <a name="identifying-current-mode-in-gvim"></a>Identifying current mode in gvim

* In Insert mode, the cursor is a blinking `|` cursor, also `-- INSERT --` can be seen on left hand side of the Command Line
* In Normal mode, the cursor is a blinking rectangular block cursor, something like this █
* In Visual modes, the Command Line shows `-- VISUAL --` or `-- VISUAL LINE --` according to visual command used
* In Command Line mode, the cursor is of course on the Command Line

