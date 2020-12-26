# Supercharge your Terminal


### TMUX

An alternative to Screen, TMUX is very usefull for having several persistent sessions running on your Raspberry Pi.
```
# Update apt database
sudo apt update

# Install TMUX
sudo apt install tmux

# Creates session “default” (run the command and detach with Ctrl+b d
tmux new -ADs default
# -A option will attach to session if it already exists
# -D option will detach any client that is already attached to the session
# -s default specifies session name

# attach to default session
tmux a
```


### MOSH

A usefull protocol for connecting to your Raspberry Pi from mobile devices. Install MOSH so the server part is running on your Raspberry.
```
sudo apt install mosh
```


### VIM

Raspbian has a very lite version of VIM you may want to upgrade to a more capable one.
```
sudo apt install vim
```
