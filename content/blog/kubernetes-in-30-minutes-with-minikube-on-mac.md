+++
Categories = ["Development", "GoLang", "containers"]
Description = ""
Tags = ["Development", "golang", "containers", "kubernetes", "docker", "orchestration"]
date = "2016-08-16T17:03:53+05:30"
type = "post"
title = "Kubernetes in 30 minutes with minikube on Mac"

+++

Below are steps to create an express setup of kubernetes on your Mac for quick use.

Minikube is a small setup by Kubernetes guys, which will spawn a virtual machine and have a tiny (but fully functional) Kubernetes cluster inside the VM. `kubectl` (pronounced 'kube (like 'tube') control`) is the command line client you'll use to connect to the Kubernetes cluster (which runs inside the VM created by minikube, in case you forgot :) )

Note that you need VirtualBox installed on your system. You can do so by first installing brew cask and then installing virtualbox as outlined in the two lines below, taken from [here](https://gist.github.com/jloveland/df1bdec4705220eb5990). I installed virtualbox from a different source, but the two lines below should work without a problem:


    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew cask install virtualbox

## Minikube Installation

    curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.7.1/minikube-darwin-amd64
    chmod +x minikube
    sudo mv minikube /usr/local/bin/

Start minikube cluster

    minikube start

Just for information, config file will be located in `~/.kube/` directory, and
all the virtual machine bits will be present in `~/.minikube/` directory.

Get status of minikube cluster

    minikube status

## Install kubectl cli

Don't install `kubectl` from brew (`brew install kubernetes-cli`) as brew contains an older incompatible version of `kubectl`.

    curl -Lo kubectl http://storage.googleapis.com/kubernetes-release/release/v1.3.0/bin/darwin/amd64/kubectl
    chmod +x kubectl
    sudo mv kubectl /usr/local/bin/

List all pods from all namespaces. It should list two pods, one an 'addon-manager' and another a 'dashboard'

    kubectl get pods --all-namespaces

Populate environment variables so that docker can connect to minikube VM

    eval $(minikube docker-env)

Now list all running docker containers inside the minikube vm. This might throw error

    docker ps

If the error is `Error response from daemon: client is newer than server (client API version: 1.24, server API version: 1.23)`, then we need to install
an older version of docker along with the current docker server installation.

We'll use DVM (docker version manager) for the same, and we need to install DVM
first for that.

    brew update && brew install dvm

If brew update fails, most of the times re-running `brew update` again fixes
the problem (else do `brew upgrade && brew update` and wait for an hour for it
to finish depending upon your internet connection).

Now source the shell script for `dvm` command line tool

    [[ -s "$(brew --prefix dvm)/dvm.sh" ]] && source "$(brew --prefix dvm)/dvm.sh"

View current docker version

    dvm ls

Install older docker version 1.11.1

    export DOCKER_VERSION=1.11.1
    dvm install

Now if you do `dvm ls` you'll see that your current docker version is 1.11.1.

Now you can use `docker ps` to see all your docker containers for kubernetes
running inside minikube.

That's all for this blogpost. I'll write another blog post about how to create your own pods inside this running Kubernetes cluster.

Do let me know if you have comments/suggestions/edits/criticism/feedback.

Cheers!
Rushi
