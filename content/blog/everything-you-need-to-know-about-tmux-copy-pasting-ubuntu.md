+++
Categories = ["Development", "Productivity"]
Description = ""
Tags = ["Development", "tmux", "ubuntu", "productivity", "quickstart", "tutorial"]
date = "2016-06-16T10:59:02+05:30"
title = "Everything you need to know about Tmux copy paste - Ubuntu"
type = "post"

+++

Copying from a Tmux session is something every Tmux user struggled with once.
I'm listing out all the stuff I learnt in this blog.

I tested everything on Ubuntu 14.04 Trusty Tahr, which runs Tmux version 1.8.
To check your Tmux version, run `tmux -V`. If you have a Mac, see
[here](http://www.rushiagr.com/blog/2016/06/16/everything-you-need-to-know-about-tmux-copy-pasting/).

## Know about copy buffers
When you do a `CTRL`+`c`, the stuff you copy is stored in your computer's
buffer, called 'clipboard' from where you can paste anywhere by doing a
`CTRL`+'v'. Tmux has it's own buffer for coppying, which we'll
call 'tmux buffer'. Our goal is to understand in a Tmux session how to copy to
tmux buffer, and also to clipboard.

You can always copy stuff into clipboard while usin Tmux. "Why do I need a Tmux
buffer then", you might wonder. This is because, in your shell, the text you
want to select might not fit in your current screen (e.g. output of `cat
/etc/passwd` file). If you copy normally, you will only be able to copy text
visible on your screen, and not the output which is 'scrolled up' due to a lot
of output.

## Tmux copy-paste - the defaults
The defaults are slighly involved, so this section is purely for informational purposes, and shouldn't be memorized. Skipping this section is perfectly okay.

1. Enter 'copy mode' by pressing `CTRL`+`b`, `[`.
2. Use the arrow keys to go to the position from where you want to start copying. Press `CTRL`+`SPACE` to start copying.
3. Use arrow keys to go to the end of text you want to copy. Press `ALT`+`w` or `CTRL`+`w` to copy into Tmux buffer.
4. Press `CTRL`+`b`, `]` to paste in a possibly different Tmux pane/window.

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
For this to happen, you need to install `xclip` on your computer. Do it as:

    sudo apt-get install --assume-yes xclip

After that, you need to add this line in `~/.tmux.conf` file:

    bind -t vi-copy y copy-pipe "xclip -sel clip -i"

Now close all your tmux sessions. From now onwards, whatever you copy in Tmux
buffer will also land into system clipboard.

## Tmux copy with mouse drag!
You can enable 'mouse mode', using which you can copy text into tmux buffer by
mouse drag. For doing that, you just need to add this line to your
`~/.tmux.conf` file:

    setw -g mode-mouse on
    set -g mouse-select-window on


## But now I can't do normal copy-paste with mouse!
You'll notice that now all your selections will go to tmux buffer, and not
clipboard buffer. Of course, you can enable copying to system clipboard as
described in a section above. However, you can notice that you can't double
click to select a complete word with vi's tmux copy-pasting shortcuts + mouse
option enabled.

Just press `SHIFT` button when copying, and now you can copy as if Tmux
doesn't exist! :)


## Copy from a remote server
Install `xclip` on the remote Ubuntu/Linux server, and add the line mentioned
above (`bind -t vi-copy y copy-pipe "xclip -sel clip -i"`) to `~/.tmux.conf` on
that server. Also, pass `-X` option when making SSH connection to the server,
like so:

    ssh -X remoteuser@remotehost

And after that everything you copy into remote's Tmux buffer will get copied
over to local clipboard. Magic!

## Other information

Done! Don't forget to comment if you know something worth letting everybody
know! Thank you:)
