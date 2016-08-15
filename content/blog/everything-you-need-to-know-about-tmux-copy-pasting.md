+++
Categories = ["Development", "Productivity"]
Description = ""
Tags = ["Development", "tmux", "apple", "productivity", "quickstart", "tutorial"]
date = "2016-06-16T10:58:02+05:30"
title = "Everything you need to know about Tmux copy paste - Mac OS"
type = "post"

+++

Copying from a Tmux session is something every Tmux user struggled with once.
I'm listing out all the stuff I learnt in this blog.

I tested everything on Mac OS X El Capitan, which runs Tmux version 2.1. To
check your Tmux version, run `tmux -V`. If you have a Linux (e.g. Ubuntu) machine, go
[here](http://www.rushiagr.com/blog/2016/06/16/everything-you-need-to-know-about-tmux-copy-pasting-ubuntu/).

## Know about your terminal
The default terminal which comes with Mac is pretty limiting. Instead of
listing the limitations (and workarounds around it), I'll just say that before
you proceed, please install iTerm 2 and start using that immediately. What I'm
writing below is all for iTerm 2. (Do let me know if you absolutely need Tmux
copy-paste to work in Terminal, and I'll provide steps for that).

## Know about copy buffers
When you do a `Command`+`c`, the stuff you copy is stored in your computer's
buffer, called 'clipboard' from where you can paste anywhere by doing a
`Command`+'v'. Tmux has it's own buffer for coppying, which we'll
call 'tmux buffer'. Our goal is to understand in a Tmux session how to copy to
tmux buffer, and also to clipboard.

You can always copy stuff into clipboard while usin Tmux. "Why do I need a Tmux
buffer then", you might wonder. This is because, in your shell, the text you
want to select might not fit in your current screen (e.g. output of `cat
/etc/passwd` file). If you copy normally, you will only be able to copy text
visible on your screen, and not the output which is 'scrolled up' due to a lot
of output.

## Tmux copy-paste - the defaults
Well, the defaults in Mac are horrible. There is simply no way to copy into
tmux buffer without using any external plugin. BUT you can copy stuff into
system clipboard by just using your mouse for selecting text.

## Tmux Vim-bindings for copying into tmux buffer
Adding configuration described in this section will give you easier shortcuts
for copy-pasting in Tmux. Moreover, these shortcuts work very similar to Vim's
copy-pasting shortcuts!

1. Add these lines in a file by name `~/.tmux.conf`.

        bind P paste-buffer
        bind-key -t vi-copy 'v' begin-selection
        bind-key -t vi-copy 'y' copy-selection
        bind-key -t vi-copy 'r' rectangle-toggle

2. Now you can enter copy mode by pressing `CTRL`+`b`,`[`, and then go
   to start point, press 'v' and start copying. After you have selected text
   you want to copy, you can just press 'y' (or the default 'enter' key) to
   copy the text into Tmux's buffer. This is exactly the commands you would use
   in Vim to copy text.
3. To paste, press `CTRL`+`b`,`P`. Note that it's capital 'p' (i.e.
   `SHIFT`+`p`). This again is similar to Vim's shortcut 'p' for paste, though
   not exactly similar. You'll realize in your Tmux journey why didn't we use a
   small 'p' instead of a capital 'P' ;)

## Copy from tmux buffer to system buffer (clipboard)
In newer iTerm2, you need to enable one option in settings to copy a text into
both tmux buffer and clipboard at the same time. Go to iTerm2 > Preferences >
"General" tab, and in the "Selection" section, check the box which says
"Applications in terminal may access clipboard" if it isn't checked. See
[here](http://apple.stackexchange.com/a/248828) for a screenshot.

## Tmux copy with mouse drag!
You can enable 'mouse mode', using which you can copy text into tmux buffer by
mouse drag. For doing that, you just need to add this line to your
`~/.tmux.conf` file:

    set -g mouse on

## But now I can't do normal copy-paste with mouse!
You'll notice that now all your selections will go to tmux buffer, and not
clipboard buffer. Of course, you can enable copying to system clipboard as
described in a section above. However, you can notice that you can't double
click to select a complete word with vi's tmux copy-pasting shortcuts + mouse
option enabled.

Just press `Option` (Alt) button when copying, and now you can copy as if Tmux
doesn't exist! :)


## Copy from a remote server
Now if your remote server is a Linux machine (e.g. an Ubuntu server), then
getting that remote server's tmux buffer copied into our local Mac's clipboard
makes for an interesting story! If the iTerm setting to allow applications in
terminal to access clipboard is enabled, then if you are running a tmux session
on remote server, and you copy stuff there into the tmux buffer of that system,
the copied text is already into your local system's clipboard! Amazing! BUT but
but, this only works if you are connecting to the remote machine directly from
your terminal, i.e. not from inside a local Tmux session!! If you have a local
Tmux session, and then you SSH into remote machine, connect to remote Tmux
session and then try to copy text in the 'remote' Tmux session (i.e. using
`Ctrl`+`b` twice to send Tmux commands to inner Tmux session), then this won't
work! (Note that you can always copy in the 'outer' Tmux session, i.e. local
Tmux session, but as stated previously, you won't be able to copy text which is
'scrolled up' in the inner Tmux session.)

Generally I'm satisfied with the limitation mentioned here. In order to get
full copying from remote session working, you will need to do something more
involved, like [here](http://superuser.com/a/408374/315134). Do let me know if
you need a better explanation of that method (e.g. more details on SSH keys and
enabling remote option on Mac OS X)

## Other information

Done! Don't forget to comment if you know something worth letting everybody
know! Thank you:)
