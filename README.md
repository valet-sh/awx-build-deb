[![License](http://img.shields.io/:license-apache-blue.svg?style=flat-square)](http://www.apache.org/licenses/LICENSE-2.0.html)



WARNING: This project is in its early development stage. Do not use these packages in production!


## Introduction

This project aims to provide native Debian/Ubuntu packages for [Ansible AWX](https://github.com/ansible/awx) 


## Installation

There are two ways of getting a native AWX package. You can build the package by yourself or you can use our prebuild packages which are available on a cloudsmith debian/ubuntu repository 


### Build packages by yourself

We recommend to use the pre-built packages, but if you want to test or debug something it may makes sense to build a package for yourself. 


#### Requirements

ansible (>= 2.5) and lxd needs to be installed on your build machine. The whole build process is running inside a lxc container.


#### Building the package

For building the package, clone this repository to your build maschine an run ether the `build-debian-package.yml` or the `build-ubuntu-package.yml` ansible playbook.


```
ansible-playbook playbooks/build-debian-package.yml
```

The package will be automatically stored on your build machine in the `/tmp` directory.  


### Use pre-built packages

The Debian repository is provided by [cloudsmith](https://cloudsmith.io). Thanks for the great service! 

currently we are only providing a "testing" repository. After some feedback and more time to test we will add a "stable" repository.

import the gpg key into your system
```
wget -O - 'https://dl.cloudsmith.io/public/valet-sh/debian-testing/cfg/gpg/gpg.B63B658377885BFD.key' | sudo apt-key add -
```

add the repository 
```
echo "deb https://dl.cloudsmith.io/public/valet-sh/debian-testing/deb/debian stretch main" | sudo tee /etc/apt/sources.list.d/valet-sh-debian-testing.list
```

Update your apt cache and install AWX
```
sudo apt update
sudo apt install awx
```

## Configuration

You can find our configuration guide [here](./CONFIGURATION.md)


## License


[Apache v2](./LICENSE.md)
