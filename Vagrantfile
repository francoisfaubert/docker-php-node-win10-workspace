# To allow faster NFS sharing, the Vagrant box needs to have a fixed IP
# Change this for an IP you can use on your subnet.
VM_IP = "192.168.10.100"


unless Vagrant.has_plugin?("vagrant-docker-compose")
    raise "Please run `vagrant plugin install vagrant-docker-compose`."
end


persist_cache_directory = `echo %userprofile%\\.workspace-persist`.chomp
unless File.directory?(persist_cache_directory)
    puts "==> Creating persistant Vagrant cache directory"
    puts "==> " + persist_cache_directory
    Dir.mkdir persist_cache_directory
end

Vagrant.configure("2") do |config| 

    config.vm.box = "ubuntu/trusty64"
    config.vm.box_version = "0.0.1"
    config.vm.network :private_network, ip: VM_IP
    config.ssh.forward_agent = true

    config.vm.synced_folder "./", "/vagrant", type: "nfs"   
    config.vm.synced_folder persist_cache_directory, "/home/vagrant/.workspace-persist/"  

    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "1"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
    end

    # Copy script helpers that ease some areas of the workflow.
    config.vm.provision "file", source: "helpers/composer", destination: "composer"
    config.vm.provision "file", source: "helpers/npm", destination: "npm"
    config.vm.provision "file", source: "helpers/persist-sync", destination: "persist-sync"


    # Move the helpers to the bin directory
    config.vm.provision "shell" do |s|
      s.inline = <<-SHELL
        # echo '#!/bin/bash' > composer
        # echo 'docker run --rm --interactive --tty --volume $PWD:/app composer \"$@\"' >> composer
        sudo chmod +x composer && sudo mv composer /usr/local/bin/composer

        # echo '#!/bin/bash' > npm
        # echo 'docker run --rm --interactive --tty --volume $PWD:/usr/src/app -w /usr/src/app node npm \"$@\"' >> npm
        sudo chmod +x npm && sudo mv npm /usr/local/bin/npm

        # echo '#!/bin/bash' > persist-sync
        # echo 'if [ "$1" -eq  "pull"] ; then echo "I would pull"; fi' >> persist-sync
        # echo 'if [ "$1" -eq  "push"] ; then echo "I would push"; fi' >> persist-sync
        sudo chmod +x persist-sync && sudo mv persist-sync /usr/local/bin/persist-sync
      SHELL
    end

    # Change directory automatically on ssh login to
    # the one containing the project list
    config.vm.provision "shell" do |s|
      s.inline = <<-SHELL
        if ! grep -qF "cd /" /home/vagrant/.bashrc ;
        then echo "cd /vagrant" >> /home/vagrant/.bashrc ; fi
      SHELL
    end
        
    # Docker is used to build project dependencies
    config.vm.provision :docker do |d|

        # Prepare Composer and Node as default containers
        d.pull_images "composer:latest"
        d.pull_images "node:latest"
            
        # Install dependencies using the containers
        # when configuration files are found
        if File.exists?('composer.json')
            config.vm.provision "shell", inline: 'echo "Fetching Composer dependencies" && composer install'
        end

        if File.exists?('package.json')            
            config.vm.provision "shell", inline: 'echo "Fetching NPM dependencies" && npm install'
        end
    end

    # When the project uses docker-compose, provision using the yml file
    # Otherwise, just install docker-compose
    if File.exists?('docker-compose.yml')        
        config.vm.provision :docker_compose, yml: "docker-compose.yml", rebuild: true, run: "always"
    else
        config.vm.provision :docker_compose
    end
end