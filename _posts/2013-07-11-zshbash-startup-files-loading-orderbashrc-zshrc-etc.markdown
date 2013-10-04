---
layout: post
title: "Zsh/Bash startup files loading order(.bashrc, .zshrc etc.)"
description: "Zsh/Bash startup files loading order (.bashrc, .zshrc etc.)"
category: 
tags: [linux]
---

repost from :http://shreevatsa.wordpress.com/2008/03/30/zshbash-startup-files-loading-order-bashrc-zshrc-etc/

Modified a little by me.

If you have ever put something in a file like .bashrc and had it not work, or are confused by why there are so many different files — .bashrc, .bash_profile, .bash_login, .profile etc. — and what they do, this is for you.

The issue is that Bash sources from a different file based on what kind of shell it thinks it is in. For an “interactive non-login shell”, it reads .bashrc, but for an “interactive login shell” it reads from the first of .bash_profile, .bash_login and .profile (only). There is no sane reason why this should be so&nbsp;it’s just historical. Follows in more detail.

For Bash, they work as follows. Read down the appropriate column. Executes A, then B, then C, etc. The B1, B2, B3 means it executes only the first of those files found.


|          |Interactive login|Interactive non-login  |Script|
|----------------|-----------|-----------|------|
|/etc/profile    |   A       |           |      |
|/etc/bash.bashrc|           |    A      |      |
|~/.bashrc       |           |    B      |      |
|~/.bash_profile |   B1      |           |      |
|~/.bash_login   |   B2      |           |      |
|~/.profile      |   B3      |           |      |
|BASH_ENV        |           |           |  A   |
|~/.bash_logout  |    C      |           |      |



Typically, most users will encounter a login shell only if either:
* they logged in from a tty, not through a GUI
* they logged in remotely, such as through ssh.
If the shell was started any other way, such as through GNOME’s gnome-terminal or KDE’s konsole, then it is typically not a login shell — the login shell was what started GNOME or KDE behind your back when you logged in, things started anew are not login shells. New terminals or new screen windows you open are not login shells either. (Starting a new window in OS X’s Terminal.app seems to count as a login shell, though.)

So typically (or sooner or later), what you will encounter are non-login shells. So this case is what you should write your config files for. This means putting most of your stuff in ~/.bashrc, having exactly one of ~/.bash_profile, ~/.bash_login, and ~/.profile, and sourcing ~/.bashrc from it. If you have nothing that you specifically want to happen only for login shells, you can even symlink one of the three to ~/.bashrc. In fact, even if you do, it is probably a good idea to have only file, as follows:

##Bash customisation file

##General configuration starts: stuff that you always want executed

##General configuration ends

if [[ -n $PS1 ]]&nbsp;then
    : # These are executed only for interactive shells
    echo "interactive"
else
    : # Only for NON-interactive shells
fi

if shopt -q login_shell &nbsp;then
    : # These are executed only when it is a login shell
    echo "login"
else
    : # Only when it is NOT a login shell
    echo "nonlogin"
fi
Almost everything should go in the “general configuration” section. There might be some commands (those which produce output, etc.) that you only want executed when the shell is interactive, and not in scripts, which you can put in the first “conditional section”. I don’t see any reason to use the rest. You can drop the “echo” lines, but keep the “:”s — they are commands which do nothing, and are needed if that section is empty.

You then need to have only file, and you can call this ~/.bashrc and do cd &amp; ln -s .bashrc .bash_profile

For zsh: [Note this is not entirely correct, as zsh reads ~/.profile as well, if ~/.zshrc is not present. Will fix this later.]


|                |Interactive login|Interactive non-login|Script|
|----------------|-----------|-----------|------|
|/etc/zshenv     |    A      |    A      |  A   |
|~/.zshenv       |    B      |    B      |  B   |
|/etc/zprofile   |    C      |           |      |
|~/.zprofile     |    D      |           |      |
|/etc/zshrc      |    E      |    C      |      |
|~/.zshrc        |    F      |    D      |      |
|/etc/zlogin     |    G      |           |      |
|~/.zlogin       |    H      |           |      |
|~/.zlogout      |    I      |           |      |
|/etc/zlogout    |    J      |           |      |

Moral:
For bash, put stuff in ~/.bashrc, and make ~/.bash_profile source it.
For zsh, put stuff in ~/.zshrc, which is always executed.
