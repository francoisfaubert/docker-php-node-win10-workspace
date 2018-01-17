# Docker-ready workspace for Windows 

This Vagrant box allows one to run Docker natively on a Linux VM on a Windows host. 

Unlike Docker Toolbox, this alternative seems to offer better network speeds in between containers and offers faster NFS file sharing in between machines.

### Requirements

It has the following hard-coded assumptions:

* Host OS must be Windows 10
* This box's `Vagrantfile` must be within a project directory
* The project directory should contain a docker-compose.yml to add server components
* It expects to run PHP applications at which ever version `php:7-alpine` is using
* Git, Node.js (and NPM) must be installed on and ran from the _host_ machine

### Preloaded Docker containers

The VM will automatically provision the [Composer](https://hub.docker.com/_/composer/) and [Node](https://hub.docker.com/_/node/) containers. It also provides a simplistic level of abstraction by forwarding calls from a global binary mock to their respective containers to prevent the need to call the longer Docker command.

NPM requires Vagrant to be ran with administrative privileges for symlinks to work as intended.

## Installation

* Save [this file](https://raw.githubusercontent.com/francoisfaubert/docker-php7-win10-workspace/master/Vagrantfile) as a `Vagrantfile` saved at the root of your project's directory
* Run `vagrant up`

## Updates

* Save [this file](https://raw.githubusercontent.com/francoisfaubert/docker-php7-win10-workspace/master/Vagrantfile) over the existing `Vagrantfile` at the root of your project's directory
* Run `vagrant reload --provision`
