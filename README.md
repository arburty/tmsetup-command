# tmsetup-command

A command to set up a new tmux session.  With an optional first argument the
new session can be named, or the default 'workspace' will be used.  A
pre-existing session of the same name will be attached to.

*NOT PORTABLE* due to the use of the command 'welcome'.  This is a custom
commnad I wrote to welcome me in a new session (really when I boot my home
computer) and can be found at
[arburty/dotfiles-and-scripts][https://github.com/arburty/tmsetup-command] and
can be removed or replaced with the commented line above it.

`tmsetup [sessName]`

# tmstart

tmstart takes in pairs of arguments following the pattern `name my/path/` and
creates multiple sessions with tmsetup.  if the `-d` flag is passed first then
no session will be attached to.  Otherwise the first pair passed will be
attached.

`tmstart [-d] [sessName my/path/]... `

The `-w` flag opens a `workspace` in the home directory, and can be used in
place of any name and path pair 

`tmstart [-d] -w [sessName my/path/]...`

## tmclose

In conjunction with this command I also use a command to close sessions called
tmclose.

This command allows the user to close the current attached session by simply
using `'tmclose'` or select a seesion to close using `'tmclose sessName'` with
sessName being the name of the session or just part of it.  Similar to using
`tmux kill-session -t sessName`
