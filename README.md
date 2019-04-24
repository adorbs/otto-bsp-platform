# Description
otto-bsp-platform is intended to provide developers of the OTTO synth an easy to setup environment for OpenEmbedded/Yocto project development. A Repo (https://gerrit.googlesource.com/git-repo/) manifest is provided here to quickly source all of the required metadata to begin building an image for OTTO. In addition, a Vagrantfile is provided for those who do not already have Linux systems ready to quickly get started via a virtual machine.

# Getting started without an existing Linux system
If you do not already have a linux system available for development, you can use the supplied Vagrantfile. It will automatically set up an Ubuntu 18.10 based system with all the required dependencies and automatically download the source repository. You can download Vagrant from https://www.vagrantup.com/. Once installed, download the Vagrantfile and simply perform the following, where `<provider>` is either `vmware_fusion`, `virtualbox`, or `parallels`, based on what virtual machine software you have installed on your system. If you would like to allocate more memory and CPU resources to the virtual machine, make sure to modify the `VM_MEMORY` and `VM_CORES` lines in the Vagrantfile prior to performing the `vagrant up` command.

```
vagrant up --provider=<provider>
vagrant ssh
```

# Getting started with an existing Linux system
If you already have a linux system available for development, the dependencies for development are as follows:
```
gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev xterm repo
```

Once those are installed, you are ready to download the source:
```
git clone -b thud https://github.com/adorbs/otto-bsp-platform.git
cd otto-bsp-platform
repo init -u https://github.com/adorbs/otto-bsp-platform -b thud
repo sync
```

Once the source is downloaded, you are ready to set up your local build directory and initiate a build:
```
MACHINE=otto-proto-v1 DISTRO=poky source setup-environment build
bitbake otto-image
```

# meta-otto

The otto-bsp-platform repository mainly exists to facilitate build environment setup. The recipes which define the image build are located in the Yocto layer meta-otto, whose source may be found here: https://github.com/adorbs/meta-otto
