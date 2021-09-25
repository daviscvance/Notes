# Command-Line Interface (CLI) Shortcuts

[TOC]

Here’s a table of the shortcuts in Bash, which I hope work with zsh.

## Bash Table

Shortcuts     | Description
------------- | -------------------------------------------------------------
ctrl-a        | Go to the start of a line (Home)
ctrl-e        | Go to the end of a line (End)
ctrl-u        | Delete from cursor to the start of a line
ctrl-k        | Delete from cursor to the end of a line
ctrl-w        | Delete from cursor to start of word (werase)
ctrl-y        | Paste the last deleted text (Yank)
ctrl-f        | Move forward one character (Left)
ctrl-b        | Move backward one character (Right)
alt-f         | Move forward one word
alt-b         | Move backward one word
ctrl-d        | Delete character after cursor
ctrl-h        | Delete character before cursor (Backspace)
alt-d         | Delete word after cursor
alt-Backspace | Delete word before cursor
ctrl-t        | Swap the last two character before the cursor (Typo)
alt-t         | Swap the last two words before the cursor (Typo)
ctrl-p        | Previous command in history (Up Arrow)
ctrl-n        | Next command in history (Down Arrow)
ctrl-\_       | Undo
ctrl-r        | Search the history backwards (Recall)
ctrl-s        | Search the history forwards
ctrl-o        | Execute the command found via ctrl-r or ctrl-s
ctrl-g        | Escape from history searching mode
ctrl-l        | Clear the screen (Clear, similar to the clear command)
ctrl-s        | Stop output to the screen (for long running verbose commands)
ctrl-q        | Allow output to the screen
ctrl-c        | Exit current running (SIGINT)
ctrl-d        | Exit the current shell
ctrl-z        | Suspend the current foreground process
alt-h         | Goto manual of current command (Help)
alt-l         | List the files of current folder (Similar to the ls command)
alt-L         | Lower the case from current cursor to the word end
alt-u         | Upper capitalize from current cursor to the word end
alt-c         | Capitalize from current cursor to the word end
Tab           | Auto-complete commands
ctrl-I        | Tab (Indent)
ctrl-J        | Newline
ctrl-M        | Enter
ctrl-\[       | Escape (ESC)
ctrl-7        | ctrl-\_ (Undo)
ctrl-8        | ctrl-? (Backward-delete-char)
ctrl-xu       | Incremental undo, separately remembered for each line
ctrl-x\*      | Expand the \* when typing a command, e.g. `ls *`
!!            | Run last command
!foo          | Run last command that BEGINS with foo
!n            | Run from the last command, args n
!n:m          | Run from the last command, args n to m
!n:\$         | Run from the last command, args n to the last argument
!\^           | First argument of previous command
!\$           | Run last argument of previous command
alt-.         | Run last argument of previous command
!\*           | Run the previous command except for the last word
!foo:p        | Print out last command starting with foo
!\$:p         | Print out the word that !\$ would substitute
!\*:p         | Print out the previous command except for the last word
\$\_          | Expands to the last argument to the previous simple command
^abc          | Run previous command, replacing abc with empty
^abc^def      | Run previous command, replacing abc with def
~\[Tab][Tab]  | List all users
\$\[Tab][Tab] | List all system variables

## [VIM](https://en.wikipedia.org/wiki/Vim_\(text_editor\))

Vim is a fork of Bill Joy's vi text editor program for Unix. Vim's author, Bram
Moolenaar, based it on the source code for a port of the Stevie editor to the
Amiga (Commodore 1985) and released a version to the public in 1991.

Its an easy~ish way to edit files via CLI. Having commands makes it easier.
Might be able to resuse some bash commands here.

Shortcuts     | Description
------------- | --------------------------------------------------------------
`esc`         | Go back to the "command line" in vim
:`wq`         | Write changes, quit.
ctrl-u        | Delete from cursor to the start of a line
:`d$`         | Delete characters from the cursor to the end of the line
:[from],[to]d | Delete lines `from`, `to` where `to` can be `$` last line or #
/-m           | Move cursor to the first occurrence "-m" in the file
n             | Find the next "-m"

## Miscellaneous Stuff

Commands I found useful and dont wan't to forget although I probably won't ever
use them, I just think they are cool because I'm not a good CLI user.

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>Download a url as-is to current dir from the internet with curl</h5>

```bash
curl -O https://upload.wikimedia.org/wikipedia/commons/0/09/Eagle_nebula.png
```

</section>

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>CLI Piping (< << >> > |)</h5>

| Symbol | Utility                                                             |
| ------ | ------------------------------------------------------------------- |
| `<`    | Redirect input i.e. get something from right side.                  |
| `<<`   | Input stream literal i.e. continue to get something from right.     |
:        : `EOF` can be used to stream inputs e.g. `cat << EOF` will take user :
:        : inputs and cat the 'file'.                                          :
| `>>`   | Append output to i.e. put more of something into right side.        |
| `>`    | Redirect output to i.e. put something into right side.              |
| `\|`   | Chain commands in sequence.                                         |

</section>

## References

-   [Jeffrey Tse](https://github.com/jeffreytse/jekyll-jeffreytse-blog/blob/master/_posts/2016-10-12-bash-shortcuts-table.md)
-   [Bash Shortcuts](https://kapeli.com/cheat_sheets/Bash_Shortcuts.docset/Contents/Resources/Documents/index)
-   [How-to: Bash Keyboard Shortcuts](https://ss64.com/bash/syntax-keyboard.html)
-   [Bindable Readline Commands](https://www.gnu.org/software/bash/manual/html_node/Bindable-Readline-Commands.html)
