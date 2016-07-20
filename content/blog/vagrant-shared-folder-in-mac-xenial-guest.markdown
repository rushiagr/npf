+++
Categories = ["Development", "vagrant", "ubuntu", "virtualbox"]
Description = ""
Tags = ["Development", "trick"]
date = "2016-07-20T23:18:24+05:30"
type = "post"
title = "Make shared folders work in Vagrant for Mac OS X host with Ubuntu Xenial guest"

+++

Today I installed latest Vagrant, version 1.8.5. I was waiting for a newer
release because 1.8.1 and previous versions don't work well with host-only
networks for Ubuntu Xenial (16.04) guests on my Mac running El Capitan. But I
faced another issue now:

    mount: unknown filesystem type 'vboxsf'

This is while specifying a synced folder in my `Vagrantfile` with this line:

    config.vm.synced_folder("/Users/apple/src/myutils", "/home/ubuntu/myutils")

where [myutils](https://github.com/rushiagr/myutils) is where I keep all my
commanline shortcuts, tricks, and other shortcut-ish stuff. I Googled but
couldn't find an article which described a way to fix this in one go, hence
this blog post :)

The actual answer is pretty simple. Just install the 'vbguest' plugin.

    vagrant plugin install vagrant-vbguest

Of course I'm assuming you're running Vagrant with VirtualBox :)

After that just shut down the VM (`vagrant halt <vmname>`), and start it again
(`vagrant up <vmname>`), and everything should work as expected. I noticed two
things:
1. Vagrant tries to install guest additions inside the VM by doing some
   `apt-get` stuff. Don't worry about it and let it finish.
2. Note that unlike Ubuntu Trusty (14.04), my hostname on Ubuntu Xenial is
   `ubuntu` and not `vagrant`, so change the `synced_folder` line in your
   `Vagrantfile` accordingly.

Cheers!
Rushi
