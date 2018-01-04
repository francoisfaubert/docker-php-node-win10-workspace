# Docker-ready workspace for Windows 

This Vagrant box allows one to run Docker natively on a Linux VM on a Windows host. 

Unlike Docker Toolbox, this alternative seems to offer better network speeds in between containers and offers faster NFS file sharing in between machines.

## Requirements

It has the following hard-coded assumptions:

* Host OS must be Windows 10
* Projects This `Vagrantfile` must be within a project directory
* The project directory should contain a docker-compose.yml
* Vagrant should be ran with elevated rights to allow symlinks
* It expects to run PHP applications which use NPM, both at their most recent versions

## Preloaded Docker containers

The VM will automatically provision [Composer](https://hub.docker.com/_/composer/) and [Node](https://hub.docker.com/_/node/) containers. It provides a simplistic level of abstraction by forwarding calls from a global binary mock to their respective containers.

## Updating the Vagrant image

This process is not really automated and requires weird manual steps :

__Find the image's guid__

One of the images listed by the following command will represent the guid of the box we want to export.

~~~
$ VBoxManage list vms
"default" {3e4a58a7-cc98-4061-a216-cec267b6dd38}                                  
"workspace_default_1515009612125_43442" {52a8ec0e-6300-416c-8983-e43a9212a7ad}
~~~

__Package the VM__

~~~
$ vagrant package --base 52a8ec0e-6300-416c-8983-e43a9212a7ad --output docker-php-node-win10-workspace.box
~~~

__Upload on Vagrant's hub__

Follow the steps to upload a new version of the box on the [hub](https://app.vagrantup.com/) website.