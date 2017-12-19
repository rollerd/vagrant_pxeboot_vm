# CentOS7 Vagrant VM with plays for configuring as a PXE boot server

---
### What is this
This is a Vagrant box built with Ansible scripts to configure a PXE server with the following image options:
* Ubuntu 17.10
* Gentoo
* More images can be added by editing the `pxe_configure/tasks/main.yml` file and the `pxe_configure/defaults/main.yml` file

### Usage:
* Run `vagrant up` command
* After the server has booted and been configured via Ansible, grab the ip address from the system: `ip addr`
* I use my router running the [Tomato](https://advancedtomato.com/) firmware as the DHCP server and pass the IP of the PXE server from above with the following line in the DHCP config section:

```
dhcp-boot=pxelinux.0,,<IP address of PXE server>
```

### Other configuration
* You can also modify the Ansible plays to setup a DHCP server on the PXE server itself by uncommenting the appropriate sections in the pxe_configure/tasks/main.yml file
* You will also need to ensure that the gathered Ansible fact for the network interface name is correct or change it if not
* To limit which images are installed, edit the 'osname_image' variable in the `provisioning/provision_vagrant.yml` file

