# Docker-ready workspace for Windows 

This Vagrant box allows one to run Docker natively on a Linux VM on a Windows host. 

Unlike Docker Toolbox, this alternative seems to offer better network speeds in between containers and offers faster NFS file sharing in between machines.

## Requirements

It has the following hard-coded assumptions:

* Host OS must be Windows 10
* This box's `Vagrantfile` must be within a project directory
* The project directory should contain a docker-compose.yml
* Vagrant should be ran with elevated rights to allow symlinks
* It expects to run PHP applications which use NPM, both at their most recent versions

## Preloaded Docker containers

The VM will automatically provision [Composer](https://hub.docker.com/_/composer/) and [Node](https://hub.docker.com/_/node/) containers. It provides a simplistic level of abstraction by forwarding calls from a global binary mock to their respective containers.
