# <a name="normal-mode"></a>Normal mode

* [Cut](#cut)
* [Copy](#copy)
* [Paste](#paste)
* [Undo](#undo)
* [Redo](#redo)
* [Replace characters](#replace-characters)
* [Repeat](#repeat)
* [Open new line](#open-new-line)
* [Indenting](#indenting)
* [Arrow Movements](#arrow-movements)
* [Moving within current line](#moving-within-current-line)
* [Word and Character based move](#word-and-character-based-move)
* [Text Objects based move](#text-objects-based-move)
* [Moving within current file](#moving-within-current-file)
* [Scrolling](#scrolling)
* [Marking frequently used locations](#marking-frequently-used-locations)
* [Jumping back and forth](#jumping-back-and-forth)
* [Numbers](#numbers)
* [Multiple copy-paste using " registers](#multiple-copy-paste)
* [Special registers](#special-registers)
* [Combining editing commands with movement commands](#combining-editing-commands-with-movement-commands)
* [Context editing](#context-editing)
* [Miscellaneous](#miscellaneous)

Remember to be in Normal mode, if not press `Esc` key. Press `Esc` again if needed

<br>
### <a name="cut"></a>Cut

There are various ways to delete text, which can then be pasted elsewhere using paste command

* Select text (using mouse or visual commands), press `d` to delete the text
* `dd` delete current line
* `2dd` delete current line and the line below it - total 2 lines
    * `dj` or `d↓` can also be used (`j` and `↓` both function as down arrow key)
* `10dd` delete current line and 9 lines below it - total 10 lines
* `dk` delete current line and the line above it
    * `d↑` can also be used
* `D` delete from current character to end of line
* `x` delete only the current character under cursor
* `5x` delete character under cursor and 4 characters to its right - total 5 characters
* `cc` delete current line and change to Insert mode
* `4cc` delete current line and 3 lines below it and change to Insert mode - total 4 lines
* `C` delete from current character to end of line and change to Insert mode
* `s` delete only the character under cursor and change to Insert mode
* `5s` delete character under cursor and 4 characters to its right and change to Insert mode - total 5 characters
* `S` delete current line and change to Insert mode (same as `cc`)

<br>
### <a name="copy"></a>Copy

There are various ways to copy text using the **yank** command y

* Select text to copy (using mouse or visual commands), press `y` to copy the text
* `yy` copy current line
* `Y` also copies current line
* `y$` copy from current character to end of line
* `2yy` copy current line and the line below it - total 2 lines
    * `yj` and `y↓` can also be used
* `10yy` copy current line and 9 lines below it - total 10 lines
* `yk` copy current line and the line above it
    * `y↑` can also be used

<br>
### <a name="paste"></a>Paste

The **paste** command `p` is used after cut or copy operations

* `p` paste the copied content one time
    * If copied text was line(s) paste **below** the current line
    * If the copied text was less than a line, paste the content to **right** of the cursor
* `P` paste the copied content one time
    * If copied text was line(s) paste **above** the current line
    * If the copied text was less than a line, paste the content to **left** of the cursor
* `3p` paste the copied content three times

<br>
### <a name="undo"></a>Undo

* `u` undo last edit
    * press `u` again for further undos
* `U` undo changes on current line
    * press `U` again to redo changes

<br>
### <a name="redo"></a>Redo

* `Ctrl+r` to redo a change undone by `u`

<br>
### <a name="replace-characters"></a>Replace characters

Often, we just need to change one character (ex: changing i to j)

* `rj` replace the charcter under cursor with j
* `ry` replace the character under cursor with y
* `3ra` replace character under cursor and 2 characters to the right with aaa

To replace mutliple characters with different characters, use uppercase `R`

* `Rlion` `Esc` replace character under cursor and 3 characters to right with lion. `Esc` key marks the completion of `R` command

The advantage of `r` and `R` commands is that one remains in Normal mode, without the need to switch to Insert mode and back

<br>
### <a name="repeat"></a>Repeat

* `.` the dot command repeats the last change
    * If the last command was deleting two lines and dot key is pressed, two more lines will get deleted
    * If last command was to select five characters and delete them, dot key will select five characters and delete
    * If the last change was clearing till end of word and inserting some text, dot key will repeat that change

<br>
### <a name="open-new-line"></a>Open new line

* `o` open new line below the current line and change to Insert mode
* `O` open new line above the current line and change to Insert mode

<br>
### <a name="indenting"></a>Indenting

* `>>` indent current line
* `3>>` indent current line and two lines below
* `<<` unindent current line
* `5<<` unindent current line and four lines below

<br>
### <a name="arrow-movements"></a>Arrow Movements

The four arrow keys can be used in Vim to move around as in any other text editor. Vim also maps them to four characters in Normal mode (faster typing compared to arrow keys)

* `h` left ←
* `j` down ↓
* `k` up ↑
* `l` right →

<br>
### <a name="moving-within-current-line"></a>Moving within current line

* `0` move to beginning of current line - column number 1
* `^` move to beginning of first non-blank character of current line, useful for indented lines
* `$` move to end of current line
* `g_` move to last non-blank character of current line
* `3|` move to 3rd column character

Moving within long lines(spread over multiple screen lines)

* `g0` to move to beginning of current screen line
* `g$` to move to end of current screen line
* `gm` to move to middle of current screen line
* `g^` to move to first non-blank character of current screen line

See `:h left-right-motions` for more info

<br>
### <a name="word-and-character-based-move"></a>Word and Character based move

Difference between word and WORD (definitions quoted from `:h word`)

> **word** A word consists of a sequence of letters, digits and underscores, or a sequence of other non-blank characters, separated with white space (spaces, tabs, <EOL>).  This can be changed with the 'iskeyword' option.  An empty line is also considered to be a word.

> **WORD** A WORD consists of a sequence of non-blank characters, separated with white space.  An empty line is also considered to be a WORD.

**Word based move**

* `w` move to start of next **word** (192.1.168.43 requires multiple `w` movements)
* `W` move to start of next **WORD** (192.1.168.43 requires one `W` movement)
* `b` move to beginning of current **word** if cursor is not at start of **word** or beginning of previous **word**
* `B` move to beginning of current **WORD** if cursor is not at start of **WORD** or beginning of previous **WORD**
* `e` move to end of current **word** if cursor is not already at end of **word** or end of next **word**
* `E` move to end of current **WORD** if cursor is not already at end of **WORD** or end of next **WORD**
* `3w` move 3 words forward, similarly number can be prefixed for `W,b,B,e,E`

**Character based move**

* `f(` move forward in the current line to next occurrence of character (
* `fb` move forward in the current line to next occurrence of character b
* `3f"` move forward to third occurrence of character " in current line
* `t;` move forward in the current line to character just before ;
* `3tx` move forward to character just before third occurrence of character x in current line
* `Fa` move backward in the current line to character a
* `Ta` move backward in the current line to character just after a
* `;` repeat previous `f,F,t,T` movement in forward direction
* `,` repeat previous `f,F,t,T` movement in backward direction

<br>
### <a name="text-objects-based-move"></a>Text Objects based move

* `(` move backward a sentence
* `)` move forward a sentence
* `{` move backward a paragraph
* `}` move forward a paragraph
* `:h object-motions` for more info

<br>
### <a name="moving-within-current-file"></a>Moving within current file

* `gg` move to first non-blank character of first line of file
* `G` move to first non-blank character of last line of file
* `5G` move to first non-blank character of fifth line of file, similarly `10G` for tenth line and so on
* `:5` move to first non-blank character of fifth line of file, similarly `:10` for tenth line and so on
* `%` move to matching pair of brackets like `() {} []` (Nesting is taken care)
    * To add a new matching pair like <>, add `set matchpairs+=<:>` to your vimrc file
    * It is also possible to match a pair of keywords like HTML tags, if-else, etc with `%` Check `:h matchit-install` for more info

<br>
### <a name="scrolling"></a>Scrolling

* `Ctrl+d` scroll half page down
* `Ctrl+u` scroll half page up
* `Ctrl+f` scroll one page forward
* `Ctrl+b` scroll one page backward
* `Ctrl+Mouse Scroll` scroll one page forward or backward

<br>
### <a name="marking-frequently-used-locations"></a>Marking frequently used locations

* `ma` mark location in the file with variable a (use any of the 26 alphabets)
* `` `a`` move from anywhere in the file to exact location marked by a
    * `'a` will move to first non-blank character of the line marked by a
    * `:marks` will show existing marks
* ``d`a`` delete upto character marked by a
    * marks can be paired with any command that accept movements like `d,c,y`

<br>
### <a name="jumping-back-and-forth"></a>Jumping back and forth

This is helpful if you are moving around while editing a large file or moving between different buffers

* `Ctrl+o` navigate to previous location in jump list (can remember it by thinking o as old)
* `Ctrl+i` navigate to next location in jump list (remember by noting that i and o are next to each other in most keyboards)
* `g;` go to previous change location
* `g,` go to newer change location
* `:h jump-motions` for more info

Related

* `gi` to place the cursor at same position where it was left last time in Insert mode

<br>
### <a name="numbers"></a>Numbers

* `Ctrl+a` increment a number (decimal/octal/hex will be automatically recognized - octal is a number prefixed by `0` and hex by `0x`)
* `Ctrl+x` decrement a number

<br>
### <a name="multiple-copy-paste"></a>Multiple copy-paste using " registers

One can use lowercase alphabets `a-z` to save content for future use. And append content to those registers by using corresponding uppercase alphabets `A-Z` at later stage

* `"ayy` copy current line to `"a` register
* `"ap` paste content from `"a` register
* `"Ayj` append current line and line below it to `"a` register (`"a` has total 3 lines now)
* `"cyiw` copy word under cursor to `"c` register

<br>
### <a name="special-registers"></a>Special registers

Vim has special purpose registers with pre-defined behavior

* `"` all yanked/deleted text is stored in this register. `p` command can also be invoked as `""p`
* `"0` yanked text is stored in this register
    * Useful for this sequence: yanking content, deleting something and then pasting yanked content using `"0p`
* `"1` to `"9` deleted contents are stored in these registers and get shifted with each new deletion. `"1` has the most recently deleted content
    * `"2p` paste content deleted before the last deletion
* `"+` this register stores system clipboard contents
    * `gg"+yG` copy entire file contents to clipboard
    * `"+p` paste content from clipboard
* `"*` this register stores visually selected text
    * contents of this register can be pasted using middle mouse button click or `"*p`

**Further reading**

* [how to use Vim registers](https://stackoverflow.com/questions/1497958/how-do-i-use-vim-registers)
* [Using registers on Command Line mode](https://stackoverflow.com/questions/3997078/how-to-paste-yanked-text-into-vim-command-line/3997110#3997110)
* [Advanced Vim registers](https://sanctum.geek.nz/arabesque/advanced-vim-registers/)

<br>
### <a name="combining-editing-commands-with-movement-commands"></a>Combining editing commands with movement commands

* `dG` delete from current line to end of file
* `dgg` delete from current line to beginning of file
* ``d`a`` delete from current character upto location marked by a
* `d%` delete upto matching pairs `() {} []`
* `ce` delete till end of word and change to Insert mode
* `yl` copy character under cursor
* `d)` delete upto end of sentence in forward direction

<br>
### <a name="context-editing"></a>Context editing

We have seen movement using `w,%,f` etc. They require precise positioning to be effective. Vim provides a way to modify commands that accepts movement like `y,c,d` to recognize context. These are `i` and `a` text object selections - easy way to remember their subtle differences is to think of `i` as **inner** and `a` as **around**

* `diw` delete a word regardless of where the cursor is on that word. Equivalent to using `de` when cursor is on first character of the word
* `diW` delete a WORD regardless of where the cursor is on that word
* `daw` delete a word regardless of where the cursor is on that word and a space character to left/right of the word depending on its position as part of sentence
* `dis` delete a sentence regardless of where the cursor is on that sentence
* `yas` copy a sentence regardless of where the cursor is on that sentence and a space left/right
* `cip` delete a paragraph regardless of where the cursor is on that paragraph and change to Insert mode
* `di"` delete all characters within pair of double quotes, regardless of where cursor is within quotes
* `da'` delete all characters within pair of single quotes as well as the quotes
* `ci(` delete all characters within () and change to Insert mode. Works even if matching parenthesis are spread over multiple lines
* `ya}` copy all characters within {} including the {}. Works even if matching braces are spread over multiple lines

For more info `:h text-objects`

<br>
### <a name="miscellaneous"></a>Miscellaneous

* `gf` open the file pointed by file path under the cursor
    * `:h gf` and `:h 'suffixesadd'` for more info
* `*` searches the word under the cursor in forward direction - matches only the whole word
    * `shift+left mouse click` can also be used in gvim
* `g*` searches the word under the cursor in forward direction - matches as part of another word also
* `#` searches the word under the cursor in backward direction - matches only the whole word
* `g#` searches the word under the cursor in backward direction - matches as part of another word also
* `Ctrl+g` display file information like name, number of lines, etc at bottom of screen
    * for more info and related commands, see `:h CTRL-G`
* `J` join current and next line with one space character in between
* `gJ` join current and next line without any character in between
* `3J` join current and next two lines with space in between the lines
* `~` invert the case of character under cursor, i.e lowercase becomes UPPERCASE and vice versa
* `g~` followed by motion command to invert case of those characters
* `gu` followed by motion command to change case of those characters to lowercase
    * example: `gue` , `gu$` , `guiw` etc 
* `gU` followed by motion command to change case of those characters to UPPERCASE
    * example: `gUe` , `gU$` , `gUiw` etc 
* `=` followed by motion command to indent code
    * example: `=}` , `gg=G` etc 
