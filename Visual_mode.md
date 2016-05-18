# <a name="visual-mode"></a>Visual Mode

* [Selection](#selection)
* [Editing](#editing)
* [Indenting](#indenting)
* [Changing Case](#changing-case)
* [Further Reading](#further-reading)

Visual mode is used to edit text by selecting them first  
Selection can either be done using mouse or using visual commands  

<br>
### <a name="selection"></a>Selection

* `v` start visual selection, use any movement command to complete selection
    * `ve, vip, v)` etc
* `V` visually select current line
    * `Vgg, ggVG` etc
* `Ctrl+v` visually select column(s)
    * `Ctrl+v3j2l` to select a 4x3 block, etc

<br>
### <a name="editing"></a>Editing

* `d` delete the selected text
* `c` clear the selected text and change to Insert mode
    * for visually selected column, after `c` type any text and press `Esc` key. The typed text will be repeated across all lines
* `I` after selecting with `Ctrl+v`, press `I`, type text and press `Esc` key to replicate text across all lines to left of the selected column
* `A` after selecting with `Ctrl+v`, press `A`, type text and press `Esc` key to replicate text across all lines to right of the selected column
* `ra` replace every character of the selected text with a
* `:` perform Command Line mode editing commands like `g,s,!,normal` on selected text
* `J` join selected lines with space character in between
* `gJ` join selected lines without any character in between
* `:h visual-operators` for more info

<br>
### <a name="indenting"></a>Indenting

* `>` indent the visually selected lines
* `3>` indent the visually selected lines three times
* `<` unindent the visually selected lines
* `=` indent code, taking care of nesting too. Example below

```
for(;;)
{
for(;;)
{
statements
}
statements
}
```

* `vip=` to indent the code

```
for(;;)
{
    for(;;)
    {
        statements
    }
    statements
}
```

<br>
### <a name="changing-case"></a>Changing Case

* `~` invert the case of visually selected text, i.e lowercase becomes uppercase and vice versa
* `U` change visually selected text to UPPERCASE
* `u` change visually selected text to lowercase

<br>
### <a name="further-reading"></a>Further Reading

* For more info on Visual mode, `:h visual-mode`
* [Visual mode tutorial](https://danielmiessler.com/study/vim/#visual)
* [Visual mode examples](https://stackoverflow.com/a/1218429/4082052)
