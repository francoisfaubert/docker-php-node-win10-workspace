unless Vagrant.has_plugin?("vagrant-docker-compose")
    raise "Please run `vagrant plugin install vagrant-docker-compose`."
end

persist_cache_directory = `echo %userprofile%\\.docker-php7-win10-workspace`.chomp
unless File.directory?(persist_cache_directory)
    puts "==> Creating persistant Vagrant cache directory"
    puts "==> " + persist_cache_directory
    Dir.mkdir persist_cache_directory
end

Vagrant.configure("2") do |config| 
    config.vm.box = "ubuntu/trusty64"
    config.vm.network :private_network, ip: "192.168.10.100"
    config.ssh.forward_agent = true

    config.vm.synced_folder "./", "/vagrant", type: "nfs"   
    config.vm.synced_folder persist_cache_directory, "/home/vagrant/.persist-cache/"
    
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "1"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
    end

    config.vm.provision "shell", inline: "curl -fsS https://raw.githubusercontent.com/francoisfaubert/docker-php7-win10-workspace/master/scripts/provision | bash"

    config.vm.provision "docker", images: ["composer:latest"]    
	config.vm.provision :docker_compose
end