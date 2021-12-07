# <a name="recording-macro"></a>Recording Macro

* [Need for Macro](#need-for-macro)
* [Example 1](#example-1)
* [Example 2](#example-2)
* [Example 3](#example-3)
* [Further Reading](#further-reading)

<br>

### <a name="need-for-macro"></a>Need for Macro

The repeat command can only repeat the last change. It also gets overwritten with every editing command. The `q` command allows user to record any sequence of editing commands to effectively create a user defined command, which can then be applied on other text across files. It can also be prefixed with a number to repeat the command. Powerful indeed!

Steps

* Press `q` to start recording session
* Use any alphabet to store the recording, say `a`
* Use the various editing commands in combination with movement commands to accomplish the sequence of editing required
* Press `q` again to stop recording
* Use `@a` (the alphabet typed in step 2) to execute the recorded command elsewhere
    * `5@a` execute 5 times
    * `@@` execute last recorded macro

<br>

### <a name="example-1"></a>Example 1

* `qwceHello<Esc>q`
    * `q` start recording
    * save recording to `w` register
    * `ce` change till end of word
    * `Hello` literally type these characters
    * `<Esc>` denotes pressing `Esc` key, one can type it in Insert or Command Line mode by pressing `Ctrl+v` followed by pressing `Esc` key
        * Note-1: It looks like `^[` but is a single character
        * Note-2: if you are using `gvim` and `Ctrl+v` is mapped to something else, `Ctrl+q` can be used instead
    * `q` stop recording
    * Now, in Normal mode, wherever you press `@w`, it will clear text until end of word and insert the characters `Hello`
    * To use the last recorded macro, `@@` can be used without having to remember which register was used to save the recording

What if you made a mistake and you wanted `'Hello!'` instead of `Hello`? Instead of meticulously recording the sequence again, we can take advantage of the fact that we are using registers to record and play the macro

* `"wp` will paste contents of `w` register, which is `ceHello<Esc>`
    * Note that in Vim, it will look like `ceHello^[`
* change it to `ce'Hello!'<Esc>`
* then visually text the sequence and press `"wy` to replace the contents of `w` register to this new sequence
    * this can be achieved in non-visual mode as well by placing cursor at start of sequence and then `"wy` followed by appropriate movement to copy till end of sequence
* now `@w` will clear text until end of word and insert the characters `'Hello!'`
* to use this recording in a portable manner, add `let @w = "ce'Hello!'<Esc>"` to `~/.vimrc`
    * Note again that you need to type the `<Esc>` key using `Ctrl+v` followed by `Esc` key in Insert mode

Now, suppose, you wanted to change only the starting word of a line, irrespective of where the current position of cursor is

* `^ce'Hello!'<Esc>`

How about changing starting word of multiple lines bunched together?
    
* `^ce'Hello!'<Esc>j` we need to first end the recording by going to next line
* `5@w` whatever count of lines to modify

Hmm, all this is fine, but how to change only those lines in the file whose starting word is `Hi` to `'Hello!'` And they are not bunched next to each other

* `:g/^Hi/ normal @w` use the `:g` command to filter the lines and then execute the Normal mode command `@w`
* Note: This particular editing can easily be done using `:% s/^Hi/'Hello!'/` This example was used to only show how to use the `q` macro recording

<br>

### <a name="example-2"></a>Example 2

Suppose, we forgot braces in Perl for single statement control structures

```perl
if($a > 4)
    print "$a is greater than 4\n";

# some other code here

if($word eq reverse($word))
    print "$word is a palindrome\n";
```

* `o{<Esc>jo}<Esc>` record the sequence for one statement in a register say `p`
* `:g/\<if(/ normal @p` execute the macro for all occurences
* When dealing with multiple lines, recording macro with `q` might be cleaner than using `:s`

```perl
if($a > 4)
{
    print "$a is greater than 4\n";
}

# some other code here

if($word eq reverse($word))
{
    print "$word is a palindrome\n";
}
```

<br>

### <a name="example-3"></a>Example 3

Suppose we need to convert these lines in markdown text

```
# <a name="intro-guide"></a>Intro Guide
# <a name="basic-steps"></a>Basic steps
# <a name="advanced-usage"></a>Advanced Usage
# <a name="further-reading"></a>Further Reading
```

to table of contents, like this

```
* [Intro Guide](#intro-guide)
* [Basic steps](#basic-steps)
* [Advanced Usage](#advanced-usage)
* [Further Reading](#further-reading)
```

* `$T>d$0r*la[<Esc>pa]<Esc>lcf"(#<Esc>f"C)<Esc>j` say this is saved in `b` register
    * `$` move to end of line
    * `T>` move to character after last occurence of '>' in the line
    * `d$` delete upto end of line
    * `0` move to first column of line
    * `r*` replace '#' with '*'
    * `l` move cursor one character right
    * `a[<Esc>` append '[' and change to Normal mode
    * `p` paste the earlier deleted content
    * `a]<Esc>` append ']' and change to Normal mode
    * `lcf"(#<Esc>` move cursor one character right, clear text upto next double quote, insert '(#' and change to Normal mode
    * `f"C)<Esc>j` move cursor to next double quote, clear text till end of line, insert ')', change to Normal mode and move cursor one line down
* `4@b` on first line, as they are bunched together
    * or `vip: normal @b`
* Note: again, this is a demonstration and `s/\v.*"(.*)".*\>(.*)/* [\2](#\1)/` is perhaps better option

<br>

### <a name="further-reading"></a>Further Reading

* [A Gentle Introduction to Macros](https://medium.com/usevim/vim-101-a-gentle-introduction-to-macros-db6b066e5b38)
* [Advanced vim macros](https://sanctum.geek.nz/arabesque/advanced-vim-macros/)
* [macro Q&A on vi stackexchange](https://vi.stackexchange.com/search?tab=votes&q=macro)
