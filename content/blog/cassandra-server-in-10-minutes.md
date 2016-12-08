+++
Categories = ["Development", "cassandra", "distributed", "cheatsheet"]
Description = ""
Tags = ["Development", "cassandra", "distributed", "cheatsheet"]
date = "2016-12-08T19:05:21+05:30"
type = "post"
title = "Cassandra server in 10 minutes"

+++

Want to quickly find out how a Cassandra server feels? In this blog post, we'll
create a single-node Cassandra cluster on an Ubuntu Xenial (16.04) system. It's
not really a 'cluster', but you can access the Cassandra shell `cqlsh` and try
out all of its commands. Since we are just looking for a quick try-out, we're
installing one of the latest Cassandra versions -- version 3.7. OK let's get
started.

Note: Jump straight to the last portion to get all the commands at one place
without explanation

Let's go to the temporary directory where we'll download Sun's Java. Here I am
downloading Java's 'Server JRE' file. I have it hosted in Amazon cloud which
you can use. You can bring your own JRE or JDK file too. Just make sure you
make appropriate replacements in the command.

    cd /tmp

Now actually download the JRE file

    wget http://eezydb.s3.amazonaws.com/server-jre-8u101-linux-x64.tar.gz

Create the directory where you want to expand this compressed file

    sudo mkdir -p /usr/lib/jvm

Now uncompress it

    sudo tar -xzvf server-jre-8u101-linux-x64.tar.gz -C /usr/lib/jvm

Now update system's Java pointers and make this Java the preferred Java to be
used

    sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_101/bin/java" 1
    sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_101/bin/java

You can check our Java is the preferred Java by checking Java version.

    java -version

The output should say something like `java version "1.8.0_101"`.

Add Cassandra repositories at appropriate place so that our beloved `apt-get`
utility can find the place from where to download Cassandra packages.

    echo "deb http://www.apache.org/dist/cassandra/debian 37x main" | sudo tee -a /etc/apt/sources.list
    echo "deb-src http://www.apache.org/dist/cassandra/debian 37x main" | sudo tee -a /etc/apt/sources.list

Now add the cryptographic keys at proper places. Don't ask me why do we have to
do it :)

    gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 749D6EEC0353B12C
    sudo apt-key add ~/.gnupg/pubring.gpg

Now update repository pointers, and install Cassandra!

    sudo apt-get --yes --quiet update
    sudo apt-get install --yes --quiet cassandra

Cassandra should be installed and running by now. You can check it by checking
status of Cassandra service

    sudo service cassandra status

Press `q` to exit. I don't know why they make us press `q`, but anyway..

Cassandra server is running by now. You can check that by checking the status
of `nodetool` utility.

    sudo nodetool status

The state should be `UN`, i.e. 'U'p and 'N'ormal. If it is not in this state,
then wait a few seconds and it will be.

There are a few more steps to get the `cqlsh` command line utility working.
Let's get them done too.

Install `pip` utility which will install Python packages. If you already have
`pip` utility, no need to execute this step. Note that this is just one of many
ways to install `pip`.

    sudo apt-get install --yes --quiet python-pip

Now install pip package `cassandra-driver`. You can install it in virtual
environment also. You can safely ignore the last statement if you have never
heard the term 'Python virtual environments' before.

    sudo pip install cassandra-driver

I wish everything worked after this, but it won't. Just set this environment
variable and then you should be good to log in into `cqlsh` shell.

    export CQLSH_NO_BUNDLED=true

Now 'actually' log into `cqlsh` shell

    cqlsh

The output should be something like:

    ubuntu@hostname:~$ cqlsh
    Connected to Test Cluster at 127.0.0.1:9042.
    [cqlsh 5.0.1 | Cassandra 3.7 | CQL spec 3.4.2 | Native protocol v4]
    Use HELP for help.
    cqlsh>

Voila! Note that you need to export `CQLSH_NO_BUNDLED=true` in every terminal
shell from where you want to access `cqlsh`. So if you log out of the system
and log back in, you need to re-run this command.

Now you can follow any `cqlsh` tutorial to try out commands to create
keyspaces, tables, rows, etc.

If you like this, please share! If you have any suggestions, please comment!

Until next time!
