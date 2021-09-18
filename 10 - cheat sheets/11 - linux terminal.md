# linux terminal

## nano

### file handling

* `Ctrl + S` - save
* `Ctrl + O` - save as
* `Ctrl + X` - exit

### cursor movement

* `Ctrl + A` - start of line

* `Ctrl + E` - end of line

* `Ctrl + Y` - one page up

* `Ctrl + V` - one page down

### searching

* `Ctrl + Q` - backward search

* `Ctrl + W` - forward search

* `Alt + Q` - find next occurrence backward

* `Alt + W` - find next occurrence forward

### editing

* `Alt + A` - turn mark on/off

* `Ctrl + K` - cut

* `Alt + 6` - copy

* `Ctrl + U` - paste

* `Alt + U` - undo

* `Alt + E` - redo

* `Ctrl + K` - remove line

### macros

* `Alt + :` - start / stop macro

* `Alt + ;` - replay macro

### misc

* `Alt + N` - toggle line numbers

## vim

### file handling

* `:wq` - write and quit
* `:q!` - quit and abandon changes

### editing

* `i` - change to insert mode at current cursor
* `a` - change to insert mode after current cursor
* `o` - insert a new line after and change to insert mode
* `O` - insert a new line before and change to insert mode
* `u` - undo
* `p` - paste into new line underneath cursor
* `P` - paste into current line, move everything afterwards 1 line down
* `Esc` - exit mode

### cursor movement

* `b` - back
* `B` - back with punctuation
* `w` - beginning of word or punctuation
* `W` - beginning of word w/ punctuation
* `e` - end of word, w/out punctuation
* `E` - end of word, w/ punctuation
* `0` - start of line
* `^` - start of line after whitespace
* `$` - end of line
* `gg` - first line
* `G` - last line
* `:n` - go to line *n*
* `/text` - search for *text*
* `n` - next occurance
* `N` - last occurance

