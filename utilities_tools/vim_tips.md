# VIM

Normal Mode = main mode that isn't in insertion mode.

## Normal Mode

append wherever the cursor is and set to Insert Mode  

- 'a'  

append to end of line and set to Insert Mode  

- Shift + 'a' (Capital A)  

undo  

- 'u'  

creates a new line and set to Insert Mode  

- 'o'  

creates a new line above and set to Insert Mode  

- Shift + 'o' (Capital O)  

delete a full line 

- 'dd' deletes a full line  

delete a single character

- 'x' deletes one character  

### Navigation

`h`
goes one character to the left

`j`
goes one character down

`k`
goes one character up

`l`
goes one character to the right

<num_of_characters or lines> [h,j,k,l] 
moving number of characters or lines up or down, left or right

#### Word Navigation

w` 
moves to the next word

`e` 
moves to the end of the word

<num_of_words>+`w`
Moves the number of words in num_of_words

**Example:**
`4`+`w`
Will move 4 words on a line
`4`+`e`
Will move 4 words from the end of the line
``
`(` 
moves to previous sentence

`)`
moves to next sentence

**Example:**
`2`+`(` 
Will move 2 sentences from cursor to previous

`{`
moves to previous paragraph

`}` moves to the next paragraph 

**Example:**
`2`+`}`
Will move 2 next paragraphs from cursor

#### File NAVIGATION

`Shift` + `g` or `G`
Going to the end of the file

`gg`
Going to the beginning of the file

`Ctrl`+`f`
Going "forward" to the next page

`Ctrl` + `b`
Going "backward" to the previous page

`:[line]`
Go to a line

`$`
Go to end of line

`0`
Go to beginning of line

`vim hello.txt +<line_number>`
Starting a file at a specific line number

### Searching

- '?' + [string to search for]
search for the first [string] (which is backwards)  

- '/' + <string to search for]  
search for the next [string] (forward)  


`n`
continue to the next searched [string]

`*`
will also do the same thing (search next)
it also finds the next instance of the word your cursor is currently on

`N`
continue to the previous searched [string]

`#` will also do the same thing (search previous)
it also finds the previous instance of the word your cursor is currently on

### Regular Expressions
`.` - matches any character

### Copy and Paste

copy a whole line  

- 'yy' (means yanking a line)  

paste a whole line below  

- 'p'  

paste a whole line above  

- Shift + 'p' (Capital P)  

## Visual Mode

Visual Mode - copy one word by selecting  

- 'y' for the selected area  

Split screen in editor:  
  sp <full_path_of_file>  

## Other - Normal Mode

View line numbers

- 'set number'

**Syntax Coloring**

To create syntax coloring, create a .vimrc file and populate the file with:  

```
set nocompatible  
syntax on  
filetype on  
filetype plugin on  
filetype indent on  
```

**Color Themes**

  Can use special vim themes/colors by searching for .vim theme files. We can add them to the above .vimrc and them to a ~/.vim/colors/ directory:  
colorscheme _name_of_color_theme_scheme  

Can also add .vim/bundle or other plugins  
and then look at how to include them in your .vimrc file

# Resources

Fun Game to practice vim:
  https://vim-adventures.com/
