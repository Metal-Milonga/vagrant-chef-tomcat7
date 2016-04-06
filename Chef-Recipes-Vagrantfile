# -*- mode: ruby -*-
# vi: set ft=ruby :
 
$myscript = <<MYSCRIPT
echo "Copying CentOS Base repo to /etc/yum.repos.d"
sudo cp -f /vagrant/CentOS-Base.repo /etc/yum.repos.d
sudo ls -l /etc/yum.repos.d/CentOS-Base.repo
#yum install -y git
#yum install -y nano
sudo service iptables stop
sudo chkconfig iptables off
MYSCRIPT

$verifyscript = <<VERIFYSCRIPT
hostname
which git
git --version
which nano
nano --version | head -n 1
which vim
vim --version | head -n 1
#mysql -u root -e "SHOW DATABASES";
VERIFYSCRIPT

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "hansode/centos-6.5-x86_64"
:wq
  config.vbguest.auto_update = false

  config.vm.provision "file",
	  source: "/home/dholton/Dropbox/Work/learning/Udemy/Vagrant-Up/vagrant/files/git-config",
          destination: "~/.gitconfig"

  config.vm.provision "file",
	  source: "/home/dholton/Dropbox/Work/learning/Udemy/Vagrant-Up/vagrant/files/CentOS-Base.repo",
          destination: "/vagrant/CentOS-Base.repo"

  config.vm.provision "shell",
	  inline: $myscript

  config.vm.provision "shell",
	  path: "https://raw.githubusercontent.com/Metal-Milonga/vagrant/master/scripts/centos-common.sh"

  config.omnibus.chef_version = :latest

  config.vm.network "forwarded_port", guest: 8080, host: 8083


  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |vb|
     # Customize the amount of memory on the VM:
     vb.memory = "1024"
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "chef_solo" do |chef|
	  chef.cookbooks_path = ["/home/dholton/Dropbox/Work/learning/Udemy/Vagrant-Up/vagrant/chef/supermarket","/home/dholton/Dropbox/Work/learning/Udemy/Vagrant-Up/vagrant/chef/cookbooks"]
	  # chef.roles_path = "/home/dholton/Dropbox/Work/learning/Udemy/Vagrant-Up/vagrant/chef/roles"
	  chef.add_recipe "java"
	  chef.add_recipe "vagrant_tomcat"
	  # chef.add_role "web"
	  chef.json = {
		  "java" => {
			  "jdk_version" => "7"
		  }
	  }
  end

end
