# <a name="regular-expressions"></a>Regular Expressions

* [Flags for Search and Replace](#flags-for-search-and-replace)
* [Pattern atom](#pattern-atom)
* [Pattern Qualifiers](#pattern-qualifiers)
* [Character Classes](#character-classes)
* [Multiple and Saving Patterns](#multiple-and-saving-patterns)
* [Word Boundary](#word-boundary)
* [Search Pattern modifiers](#search-pattern-modifiers)
* [Changing Case using Search and Replace](#changing-case-using-search-and-replace)
* [Delimiters in Search and Replace](#delimiters-in-search-and-replace)
* [Further Reading](#further-reading)

<br>

### <a name="flags-for-search-and-replace"></a>Flags for Search and Replace

* `g` replace all occurrences within line
* `c` ask for confirmation before each replacement
* `i` ignore case for searchpattern
* `I` don't ignore case for searchpattern

Flags can be combined

* `s/cat/Dog/gi` replace every occurrence of cat (ignoring case, so it matches Cat, cAt, etc) with Dog (note that `i` doesn't affect the replacement string Dog)

For more info, `:h :s_flags`

<br>

### <a name="pattern-atom"></a>Pattern atom

* `^` start matching from beginning of a line
    * `/^This` match This only at beginning of line
* `$` match pattern should terminate at end of a line
    * `/)$` match ) only at end of line
    * `/^$` match empty line
* `.` match any single character, excluding new line
    * `/c.t` match 'cat' or 'cot' or 'c2t' or 'c^t' but not 'cant'

For more info, `:h pattern-atoms`


<br>

### <a name="pattern-qualifiers"></a>Pattern Qualifiers

* `*`greedy match preceding character 0 or more times
    * `/abc*` match 'ab' or 'abc' or 'abccc' or 'abcccccc' etc
* `\+` greedy match preceding character 1 or more times
    * `/abc\+` match 'abc' or 'abccc' but not 'ab'
* `\?` match preceding character 0 or 1 times (`\=` can also be used)
    * `/abc\?` match 'ab' or 'abc' but not 'abcc'
* `\{-}` non-greedy match preceding character 0 or more times
    * Consider this line of text 'This is a sample text'
    * `/h.\{-}s` will match: 'his'
    * `/h.*s` will match: 'his is a s'
    * [Read more on non-greedy matching](https://stackoverflow.com/questions/1305853/how-can-i-make-my-match-non-greedy-in-vim)
* `\{min,max}` greedy match preceding character min to max times (including min and max)
    * min or max can be left unspecified as they default to 0 and infinity respectively
    * greedy match, tries to match as much as possible
* `\{-min,max}` non-greedy match, tries to match as less as possible
* `\{number}` match exactly with specified number
    * `/c\{5}` match exactly 'ccccc'

For more info, `:h pattern-overview`

<br>

### <a name="character-classes"></a>Character Classes

* `[abcde]` match any of 'a' or 'b' or 'c' or 'd' or 'e' ONE time
    * use `[a-e]` as shortform
* `[^abcde]` match any character other than 'a' or 'b' or 'c' or 'd' or 'e'
    * use `[^a-e]` as shortform
* `[aeiou]` match vowel character
* `[^aeiou]` match consonant character
* `\a` matches alphabet character, short-cut for `[a-zA-Z]`
* `\A` matches other than alphabet `[^a-zA-Z]`
* `\l` matches lowercase alphabets `[a-z]`
* `\L` matches other than lowercase alphabets `[^a-z]`
* `\u` matches uppercase alphabets `[A-Z]`
* `\U` matches other than uppercase alphabets `[^A-Z]`
* `\d` matches digit character `[0-9]`
* `\D` matches other than digit `[^0-9]`
* `\x` matches hexademical character `[0-9a-fA-F]`
* `\X` matches other than hexademical `[^0-9a-fA-F]`
* `\w` matches any alphanumeric character or underscore `[a-zA-Z0-9_]`
* `\W` match other than alphanumeric character or underscore `[^a-zA-Z0-9_]`
* `\s` matches white-space characters **space** and **tab**
* `\S` matches other than white-space characters
* `\t` used in replacestring to insert a Tab character
* `\r` used in replacestring to insert a newline character

For more info, `:h /character-classes`

<br>

### <a name="multiple-and-saving-patterns"></a>Multiple and Saving Patterns

* `\|` allows to specify two or more patterns to be matched
    * `/min\|max` match 'min' or 'max'
* `\(pattern\)` allows to group matched patterns and use special variables `\1`, `\2`, etc to represent them in same searchpattern and/or replacestring when using substitute command
    * `/hand\(y\|ful\)` match 'handy' or 'handful'
    * `/\(\a\)\1` match repeated alphabets

<br>

### <a name="word-boundary"></a>Word Boundary

* `\<pattern` Bind the searchpattern to necessarily be starting characters of a word
    * `/\<his` matches 'his' and 'history' but not 'this'
* `pattern\>` Bind the searchpattern to necessarily be ending characters of a word
    * `/his\>` matches 'his' and 'this' but not 'history'
* `\<pattern\>` Bind the searchpattern to exactly match whole word
    * `/\<his\>` matches 'his' and not 'this' or 'history'

<br>

### <a name="search-pattern-modifiers"></a>Search Pattern modifiers

* `\v` helps to avoid `\` for pattern qualifiers, grouping pattern, etc
    * `/\vc{5}` match exactly 'ccccc'
    * `/\vabc+` match 'abc' or 'abccc' but not 'ab'
    * `/\vabc?` match 'ab' or 'abc' but not 'abcc'
    * `/\v<his>` match whole word 'his', not 'this' or 'history'
    * `/\vmin|max` match 'min' or 'max'
    * `/\vhand(y|ful)` match 'handy' or 'handful'
    * `/\v(\a)\1` match repeated alphabets
    * `s/\v(\d+) (\d+)/\2 \1/` swap two numbers separated by space

* `\V` no need to use `\` when trying to match special characters
    * `/V^.*$` match the exact sequence ^.*$

* `\c` case insensitive search
    * `/\cthis` matches 'this', 'This', 'thiS', etc
* `\C` case sensitive search
    * `/\Cthis` match exactly 'this', not 'This', 'thiS', etc

* `\%V` only inside visually selected area
    * `s/\%Vcat/dog/g` replace 'cat' with 'dog' only in visually selected region

For more info

* `:h /magic`
* [Magicness in Vim regex](http://andrewradev.com/2011/05/08/vim-regexes/)
* [Excellent examples and other Vim settings on case sensitivity](https://stackoverflow.com/questions/2287440/how-to-do-case-insensitive-search-in-vim)

<br>

### <a name="changing-case-using-search-and-replace"></a>Changing Case using Search and Replace

These are used in the replacestring section

* `\u` uppercases the next character
* `\U` UPPERCASES the following characters
* `\l` and `\L` are equivalent for lowercase
* use `\e` and `\E` to end further case changes

Example:

* `:% s/\v(\a+)/\u\1/g` will Capitalize all the words in current file (i.e only first character of word is capitalized)
* `:% s/\v(\a+)/\U\1/g` will change to all letters to UPPERCASE in current file
* `:% s/\v(\w)_(\a+)/\1\u\2/g` change variable_name to camelCase
    * for ex: 'min_max' will change to 'minMax', 'array_sum' will change to 'arraySum' and so on
* [Changing case with regular expressions](https://stackoverflow.com/questions/17440659/capitalize-first-letter-of-each-word-in-a-selection-using-vim)

<br>

### <a name="delimiters-in-search-and-replace"></a>Delimiters in Search and Replace

One can also use other characters like `#^$` instead of `/`

* `:% s#/project/adder/#/verilog/project/high_speed_adder/#g` this avoids mess of having to use `\/` for every `/` character

<br>

### <a name="further-reading"></a>Further Reading

* `:h regular-expression`
* [vimregex](http://vimregex.com/)
* [What does this regex mean?](https://stackoverflow.com/questions/22937618/reference-what-does-this-regex-mean)
