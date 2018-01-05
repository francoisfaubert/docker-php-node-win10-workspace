# Docker-ready workspace for Windows 

This Vagrant box allows one to run Docker natively on a Linux VM on a Windows host. 

Unlike Docker Toolbox, this alternative seems to offer better network speeds in between containers and offers faster NFS file sharing in between machines.

# Installation

* Save [this file](https://raw.githubusercontent.com/francoisfaubert/docker-php-node-win10-workspace/master/Vagrantfile) to `Vagrantfile` saved the root of your project's directory
* Run `vagrant up`

# Updates

* Save [this file](https://raw.githubusercontent.com/francoisfaubert/docker-php-node-win10-workspace/master/Vagrantfile) over the existing `Vagrantfile` at the root of your project's directory
* Run `vagrant reload --provision`

## Requirements

It has the following hard-coded assumptions:

* Host OS must be Windows 10
* This box's `Vagrantfile` must be within a project directory
* The project directory should contain a docker-compose.yml
* Vagrant should be ran with elevated rights to allow symlinks
* It expects to run PHP applications which use NPM, both at their most recent versions

## Preloaded Docker containers

The VM will automatically provision the [Composer](https://hub.docker.com/_/composer/) container. It provides a simplistic level of abstraction by forwarding calls from a global binary mock to their respective containers. Node should also have been containerized, but it really does not behave well on a VM and therefore is installed on the VM through `apt-get`.
