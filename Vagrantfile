################################################################################
#
# Vagrantfile
#
################################################################################

### Change here for more memory/cores ###
VM_MEMORY=4096
VM_CORES=2

Vagrant.configure('2') do |config|
	config.vm.box = 'generic/ubuntu1810'

	config.vm.provider :vmware_fusion do |v, override|
		v.vmx['memsize'] = VM_MEMORY
		v.vmx['numvcpus'] = VM_CORES
	end

	config.vm.provider :virtualbox do |v, override|
		v.memory = VM_MEMORY
		v.cpus = VM_CORES

		required_plugins = %w( vagrant-vbguest )
		required_plugins.each do |plugin|
		  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
		end
	end

	config.vm.provider :parallels do |v, override|
		v.memory = VM_MEMORY
		v.cpus = VM_CORES
		v.update_guest_tools = true
	end

	config.vm.provision 'shell' do |s|
		s.inline = 'echo Setting up machine name'

		config.vm.provider :vmware_fusion do |v, override|
			v.vmx['displayname'] = "OTTO BSP Platform"
		end

		config.vm.provider :virtualbox do |v, override|
			v.name = "OTTO BSP Platform"
		end

		config.vm.provider :parallels do |v, override|
			v.name = "OTTO BSP Platform"
		end
	end

	# Check our system locale -- make sure it is set to UTF-8
	# This also means we need to run 'dpkg-reconfigure' to avoid "unable to re-open stdin" errors (see http://serverfault.com/a/500778)
	# For now, we have a hardcoded locale of "en_US.UTF-8"
	locale = "en_US.UTF-8"

	config.vm.provision 'shell', privileged: true, inline:
		"sed -i 's|deb http://us.archive.ubuntu.com/ubuntu/|deb mirror://mirrors.ubuntu.com/mirrors.txt|g' /etc/apt/sources.list
		dpkg --add-architecture i386
		apt-get -q update
		apt-get purge -q -y snapd lxcfs lxd ubuntu-core-launcher snap-confine
		apt-get -q -y install gawk wget git-core diffstat unzip texinfo gcc-multilib \
			build-essential chrpath socat libsdl1.2-dev xterm device-tree-compiler python repo
		apt-get -q -y autoremove
		apt-get -q -y clean
		echo 'Setting locale to UTF-8 (#{locale})...' && locale | grep 'LANG=#{locale}' > /dev/null || update-locale --reset LANG=#{locale} && dpkg-reconfigure -f noninteractive locales"

	config.vm.provision 'shell', privileged: false, inline:
		"echo 'Downloading and extracting OTTO BSP Platform'
		git clone -b thud https://github.com/adorbs/otto-bsp-platform.git
		cd otto-bsp-platform
		repo init -u https://github.com/adorbs/otto-bsp-platform -b thud
		repo sync"

end
