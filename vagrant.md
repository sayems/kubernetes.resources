# Kubernetes with Vagrant and Virtualbox


Create and configure lightweight, reproducible, and portable development environments. Vagrant is an amazing tool for managing virtual machines via a simple to use command line interface.

&nbsp;

### Install
Vagrant uses [Virtualbox](https://www.virtualbox.org/) to manage the virtual dependencies. You can [directly download virtualbox](https://www.virtualbox.org/wiki/Downloads) and install or use homebrew for it.

```
$ brew cask install virtualbox
```

Now install Vagrant either [from the website](http://www.vagrantup.com/downloads.html) or use homebrew for installing it.

```
$ brew cask install vagrant
```

[Vagrant-Manager](http://vagrantmanager.com/) helps you manage all your virtual machines in one place directly from the menubar.

```
$ brew cask install vagrant-manager
```

&nbsp;

### Usage
Now create a ```/ubuntu``` directory and cd into the ```/ubuntu``` directory. Then we'll initialize the vagrant machine.

```
$ mkdir ~/ubuntu 
$ cd ~/ubuntu
$ vagrant init ubuntu/xenial64
```

Now lets start the machine using the following command.

```
$ vagrant up
```
You can ssh into the machine now.
```
$ vagrant ssh
```

Login and password for most of Vagrant boxes are
```
Username : vagrant
Password : vagrant
```

Halt the vagrant machine now.
```
$ vagrant halt
```
Other useful commands are suspend, destroy etc.
