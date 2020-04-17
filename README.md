# tmsetup-command

Included here is a set of scripts, `tmsetup`, `tmstart`, and `tmclose`.  These work
together to create and destroy multiple tmux sessions.

1. `tmsetup`: Used to set up a new custom tmux session.
2. `tmstart`: Allows for multiple sessions to be created with `tmsetup`.
3. `tmclose`: Can destroy multiple tmux sessions at once.

# tmsetup

Makes a tmux session in a way I found myself creating almost every time. With
an optional first argument the new session can be named, or the default of
'workspace' will be used.  A pre-existing session of the same name will be
attached to.

Not usually called on the command line, as `tmstart` is more user friendly because
it can specify a directory to open all panes in, `tmsetup` uses the current working 
directory to open the panes.

By Window:

1. (workspace) The first window will have two panes, the left will have the
   cursor and be ready to go, the right pane displays the welcome command.
2. (vim) This window is ready for vim.
3. (extra) A window ready for your side tangents. (I commonly ssh and/or tail
   log files from here)
4. (ranger) Ranger, a file explorer with vi-like bindings is opened.

*NOT IMMEDIATELY PORTABLE* due to the use of the command `welcome` and the
program `ranger`.  `welcome` is a custom command I wrote to welcome me in a new
session (really when I boot my home computer) and can be found at
[arburty/dotfiles-and-scripts][https://github.com/arburty/tmsetup-command] and
can be removed or replaced with the commented line above it. Ranger can be
replaced by another file explorer, different program, or simply removed and
used as another window.

`tmsetup [-d] [sessName]`

The `-d` (detach) flag can be used to only create, and not attach to the session

# tmstart

tmstart takes in pairs of arguments following the pattern `name my/path/` and
creates multiple sessions using `tmsetup`.  The first pair given will be the
'primary' session and if no `-d` flag is passed will be the session that will
be attached to once all the sessions have been created.

`tmstart [-h --help] {[-d -p] {-w | {-s | SessionName} ./Path/}}...`

## Flags

### `-d` ) detach

If the `-d` flag is passed then no session will be attached to. Otherwise the
first pair passed will be used as the default primary session, and the one the
script will attach to.  (ex. tmux attatch -t primary)

1. `tmstart -d first ./firstpath/`

2. `tmstart second ./secondpath/ third ./thirdpath/`

3. `tmstart second ./secondpath/ -d third ./thirdpath/`

4. `tmstart second ./secondpath/ third ./thirdpath/ -d`

The first example will open a session 'first' starting in the './firstpath/'
directory and will not attach

The second example will attach to the session 'second' starting in the
'./secondpath/' directory and create 'third' in the './thirdpath/' directory

The third and fourth example are the same as example 2, but will not attach to
a session.

### `-p` ) primary

The `-p` flag will change the primary path to the next given session pair.
Useful when a long list of sessions is given and you do not want the first pair
to be the attached session. Makes no sense to use with `-d` but still works.

`tmclose first first/ projectA projectAlpha/ -p projectB projectBeta/

This will change the primary session to ProjectB, instead of using the
defaulted 'first' session.

### `-w` ) workspace

The `-w` flag opens a `workspace` session in the users home directory, and can
be used in place of a name and path pair.  Can only be used once but since A
generic session ready for tasks that are not project related is very useful it
is a nice shorthand.

`tmstart -w sessName my/path/`

This creates the 'workspace' session in the home directory and attaches to it.
It also creates a session 'sessName' in the './my/path/' directory

### `-s` ) same

The `-s` flag can be used in place of a 'name' and will use the basename of the directory

`tmstart -s my/very/longNameForAPath/`

Resulting in a session named 'longNameForAPath' starting in
'./my/very/longNameForAPath/'

### `-h`, `--help` )

Prints the USAGE message.

`tmstart -h`

`tmstart --help`

# tmclose

In conjunction with these command I also use a command to close sessions called
`tmclose`.

This command allows the user to close the currently attached session by simply
using `'tmclose'` or select many seesions to close using `'tmclose sessName...'` with
sessName being the name of the session or just part of it.  Similar to using
`tmux kill-session -t sessName`

`tmclose`

`tmclose one two three`

First example closes your current session. The second example closes 3
sessions, 'one', 'two', and 'three.'. Remember 'one' could represent a session
called 'oneMoreEdit' as only part of the name is needed.

## Flags

### `-o` ) other

Closes all 'other' sessions besides the one your are currently in.  Might not
work if multiple sessions are "attached" to, like another terminal is open and
in a different tmux session

`tmclose -o`

### `-a` ) all
Closes 'all' sessions.  Closing the other sessions first, then the session you are in last.

`tmclose -a`

### `-s` ) select

Opens a dialogue asking which sessions to remove, including options for all
"\--all", all other sessions "\--others", or exit "\--done".

`tmclose -s`
