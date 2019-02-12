# tmsetup-command

A command to set up a new tmux session.  With an optional first argument the new
session can be named, or 'workspace' will be used.  A pre-existing session of the same name will
be attached to.

*NOT PORTABLE* due to the use of the command 'figlet'.  A stupid joke I put in and can be removed.

## Useful

In conjunction with this command I also use a function declared in my *~/.bashrc* that could also be a standalone command, called tmclose():

tmclose() { if [[ ! -z $1  ]];then name=$1;else name=$(tmux list-sessions | grep attached | cut -d ":" -f 1); fi; tmux kill-session -t $name;  }

this function allows the user to close the current attached session by simply using 'tmclose' or select a seesion to close using
'tmclose sessName' with sessName being the name of the session or just part of it.  Just like using 'tmux kill-session -t sessName'
