+++
Categories = ["Development", "Productivity"]
Description = ""
Tags = ["Development", "tmux", "ubuntu", "productivity", "quickstart", "tutorial"]
date = "2016-06-16T10:58:02+05:30"
title = "Everything you need to know about Tmux copy pasting"
type = "post"

+++

Note: It is best if you actually execute steps in this tutorial along with
reading it

Estimated reading time: 30 minutes if performing a hands-on. Else 10 minutes.

Note: This Guide is written by running commands on Ubuntu Trusty (14.04), which
comes with Tmux version 1.8. For later versions of Tmux, things which are
different are mentioned at appropriate places .  You can check Tmux version
with `tmux -V` command. Please feel free to comment if you find something
missing or wrong, or know something worth adding.

### Tmux copy-paste - the defaults
I'm going to explain you the tmux defaults in this section, but don't remember
any of it! The following methods are slightly involved, so we will soon create
Tmux shortcuts for them which are easier to remember. More so, they are
Vim-like, so if you are a Vim user, you'll need to learn even less new
stuff :)

1. Enter 'copy mode'. This is the mode in which you can move the cursor
   anywhere on the screen. Do this by pressing the 'prefix' (`CTRL`+`b`), and
   then pressing `[` key. So basically press `CTRL` and then `j` while holding
   the `CTRL` button, then release both keys, and then press `[` key. Such
   sequence will be mentioned as `CTRL`+'b', `[` from now on for other such
   sequences too.
2. Now use the arrow keys to go to the start of the text you want to copy, and
   then press `CTRL`+`SPACE`, or `SHIFT`+`SPACE`. The cursor should now change
   to yellow colour, meaning it has started highlighting.
3. Now using arrow keys, move to the point till where you want to copy, and
   then press `ENTER`. Alternatively, you can press `ALT` + `w`.
4. Now you can paste the copied text by doing `CTRL`+`b`, `]`.

### Tmux Vim-bindings for copy and paste
1. Add these lines file by name ~/.tmux.conf.

        bind P paste-buffer
        bind-key -t vi-copy 'v' begin-selection
        bind-key -t vi-copy 'y' copy-selection
        bind-key -t vi-copy 'r' rectangle-toggle

2. Now you can enter copy mode as described above (`CTRL`+`b`,`[`), and then go
   to start point and press 'v' to start copying. After you have selected text
   you want to copy, you can just press 'y' to copy the text into Tmux's
   buffer. This is exactly the commands you would use in Vim to copy text.
3. To paste, press `CTRL`+`b`,`P`. Note that it's capital 'p' (i.e.
   `SHIFT`+`p`). This again is similar to Vim's shortcut 'p' for paste, though
   not exactly similar. You'll realize in your Tmux journey why didn't we use a
   small 'p' instead of a capital 'P' ;)

Before we move ahead, I want to tell you that Tmux's copy buffer is independent
of system clipboard (at least on Tmux 1.8. In Tmux 2.0 this seems to be no
longer the case on El Capitan Mac. I unfortunately don't have an Ubuntu Desktop
handy). That is, whatever you copied using `CTRL`+`c` command, it'll still be
pasted when you do a `CTRL`+`v`, and the stuff you copied in Tmux using Tmux's
way to copy stuff won't be available by default outside of that Tmux session.
But you can change that behavior. I've now started to use Mac, and somehow it
just works in it, so I don't really know how to make it work in Ubuntu any more
:(. I 'guess' if you follow steps in 'Copy from remote server', you'll get it
working. Do let me know if you are trying it!

### Tmux copy with mouse drag!
You can enable what is called as 'mouse mode'. Using it, you can just select
text by dragging mouse and making a selection. For doin that, you just need to
add these lines to your `~/.tmux.conf` file:

    setw -g mode-mouse on
    set -g mouse-select-window on

Note that this works only with Tmux version 1.8 or earlier. For later versions
of Tmux, just add this single line:

    set -g mouse on

### But now I can't do normal copy-paste with mouse!
You'll notice that now all your selections will go to tmux buffer, and not
system's buffer (called 'clipboard'). The solution is very simple -- just use
`SHIFT` button (and on Macs, use `ALT` button).

This is all what you need to know if you are using only your local computer.
But if you are a developer, there is a chance you might be dealing with remote
servers over SSH, and might want to copy text in Tmux on a remote server, and
have it accessible locally. For example you might want to copy an error message
from remote server and send it over to your colleague over chat from you local
computer. There is a way to do that too!

### Copy from a remote server
You will need to install `xclip` on the remote server and local computer too.
On Ubuntu, this can be done by doing `sudo apt-get install xclip`. Add this
line to your `~/.tmux.conf` file:

    bind -t vi-copy y copy-pipe "xclip -sel clip -i"


Then SSH on the remote server using -X option:

    ssh -X remoteuser@remotehost

Now anything you copy on that system using Tmux will come to local system's
clipboard.

Done! Don't forget to comment if you know something worth letting everybody
know! Thank you:)
