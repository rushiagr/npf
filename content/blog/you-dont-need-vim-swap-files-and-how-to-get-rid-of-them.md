+++
Categories = ["Development", "vim"]
Description = ""
Tags = ["vim", "development", "tutorial", "productivity"]
date = "2016-06-17T11:18:27+05:30"
title = "You dont need Vim swap files. And how to get rid of them"
type = "post"

+++

Estimated reading time: 5 minutes.


Almost all IDEs today have 'autosave' feature. That is, you don't need to
explicitly save a file. The file is automatically saved as you type, so that
even if your computer crashes, you don't lose data. This makes me wonder why
Vim's default behavior is of using swap files. Swap files are annoying.
I've seen all Vim developers, including me, struggle with swap files at one
point in their life.

There is a Vim plugin for autosaving, and it has saved a lot of my time. I have
used the plugin such that every time I enter normal mode (after making edits in
Insert mode), it autosaves file. And more importantly, I have disabled swap
file creation by Vim. It's really that simple folks. You don't really need swap
files 99.99% of the times.

### How to Vim autosave

Download the plugin, which is present at
[https://github.com/907th/vim-auto-save](https://github.com/907th/vim-auto-save), and put the plugin file in `~/.vim/plugin` directory:

    mkdir -p ~/.vim/plugin
    cd ~/.vim/plugin
    wget https://raw.githubusercontent.com/907th/vim-auto-save/master/plugin/AutoSave.vim

Now open the `~/.vimrc` file and add these lines. Comments are
self-explanatory.

    # Enable autosave plugin
    let g:auto_save = 1

    # Only save in Normal mode periodically. If the value is changed to '1',
    # then changes are saved when you are in Insert mode too, as you type, but
    # I would say prefer not save in Insert mode
    let g:auto_save_in_insert_mode = 0

    # Silently autosave. If you disable this option by changing value to '0',
    # then in the vim status, it will display "(AutoSaved at <current time>)" all
    # the time, which might get annoying
    let g:auto_save_silent = 1

    # And now turn Vim swapfile off
    set noswapfile


### Things to note
There are a few things to note when you switch to this 'autosave, no swap
files' mode:

1. You can't just do a `:q!` to discard unsaved changes.  Autosave already has
   saved your changes! So the only real way to discard your changes is to undo
   all the changes you've made (`u` key) and then exit the file.
2. Earlier, you can't modify a Vim file which is already open in another
   terminal. Now too Vim will throw a warning message. The difference is how
   you're notified of it. Previously, even before opening the file Vim will say
   that a swap file exists. But now, Vim will allow you to open the file, and
   start editing it too. Only when you come out of Insert mode, AND you are
   making a conflicting change, will it say:

    ```
    WARNING: The file has been changed since reading it!!!
    Do you really want to write to it (y/n)?
    ```

    A conflicting change basically means you are making edits at a place where
    edits are already made in another terminal where that same file is open.
    For example, the file initially had 'one' written on first line when it's
    opened in both terminals. In one terminal, you add a second line saying
    'two', and in another terminal when you add the second line saying 'three',
    we have a conflict. This is because we are writing 'three' at line number 2
    where 'two' is alredy written (from another terminal)

After reading these 'things to note', you might be thinking if it is really a good idea to autosave. Everybody is entitled to have an opinion. My opinion is that over a longer time period, getting rid of swap files saved me much more headache. I just have to be careful while exiting files.

I hope this helps. Questions, comments, suggestions, feedback? Comment :)

Thank you.
