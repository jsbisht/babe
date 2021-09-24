Starting `vi` editor    
    
    vi filename    edit a file named "filename"
    vi newfile     create a new file named "newfile"

## Modes

* **command mode** - The letters of the keyboard perform editing functions (like moving the cursor, deleting text, etc.). To enter command mode, press the escape  key. When a file is opened, it opens in command mode.

* **insert mode** - The letters you type form words and sentences. Press `i` to enter insert mode.

**************

## Command Mode

### Moving cursor

    w            forward word by word
    b            backward word by word
    $            to end of line
    0 (zero)     to beginning of line
    H            to top line of screen
    M            to middle line of screen
    L            to last line of screen
    G            to last line of file
    1G           to first line of file
    f            scroll forward one screen
    b            scroll backward one screen
    d            scroll down one-half screen
    u            scroll up one-half screen
    n            repeat last search in same direction
    N            repeat last search in opposite direction

### Replacing charecter
    
To replace one character with another:

1. Move the cursor to the character to be replaced.
2. Type r
3. Type the replacement character.


The new character will appear, and you will still be in command mode.

### Insert blank line

To insert a blank line below the current line, type
        
    o (lowercase)
    

To insert a blank line above the current line, type
    
    O (uppercase)


### Replacing phrase    

To change text from the cursor position to the end of the line:

1. Type C (uppercase).
2. Type the replacement text.
3. Press .

To insert text in a line:

1. Position the cursor where the new text should go.
2. Type i
3. Enter the new text.

To add text to the end of a line:

1. Position the cursor on the last letter of the line.
2. Type a
3. Enter the new text.
   
************** 

## Insert Mode 
    
    x         delete character
    nx        delete n characters
    X         delete character before cursor
    dw        delete word
    ndw       delete n words
    dd        delete line
    ndd       delete n lines
    D         delete characters from cursor to end of line
    r         replace character under cursor
    cw        replace a word
    ncw       replace n words
    C         change text from cursor to end of line
    o         insert blank line below cursor (ready for insertion)
    O         insert blank line above cursor (ready for insertion)
    J         join succeeding line to current cursor line
    nJ        join n succeeding lines to current cursor line
    u         undo last change
    U         restore current line
    

**************

## Quirks

To move the cursor to another position, you must be in command mode. If you have just finished typing text, you are still in insert mode. Go back to command mode by pressing . If you are not sure which mode you are in, press  once or twice until you hear a beep. When you hear the beep, you are in command mode.

Editing commands require that you be command mode. Many of the editing commands have a different function depending on whether they are typed as upper- or lowercase. Often, editing commands can be preceded by a number to indicate a repetition of the command.

### Closing and saving file
    
    ZZ            save file and then quit
    :w            save file
    :q!           discard changes and quit file
    :1,$d         delete file content
**************

## Reference
  
[Source](https://www.washington.edu/computing/unix/vi.html "Permalink to How to Use the vi Editor")
