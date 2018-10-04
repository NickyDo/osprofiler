# Install devstack
## Linux
Start with a clean and minimal install of a Linux system. Devstack attempts to support Ubuntu 16.04/17.04, Fedora 24/25, CentOS/RHEL 7, as well as Debian and OpenSUSE.

If you do not have a preference, Ubuntu 16.04 is the most tested, and will probably go the smoothest.

### Add Stack User
Devstack should be run as a non-root user with sudo enabled (standard logins to cloud images such as “ubuntu” or “cloud-user” are usually fine).

You can quickly create a separate stack user to run DevStack with
```bash
$ sudo useradd -s /bin/bash -d /opt/stack -m stack
```
Since this user will be making many changes to your system, it should have sudo privileges:
```bash
$ echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
$ sudo su - stack
```

### Download DevStack
```bash
$ git clone https://git.openstack.org/openstack-dev/devstack
$ cd devstack
```
The devstack repo contains a script that installs OpenStack and templates for configuration files

### Create a local.conf
* Create a local.conf file with 4 passwords preset at the root of the devstack git repo.
```bash
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```
* [For local virtual machine](/vm.md) 

### Start the install
```bash
./stack.sh
```
This will take a 15 - 20 minutes, largely depending on the speed of your internet connection. Many git trees and packages will be installed during this process.

### Profit
You now have a working DevStack! Congrats!

Your devstack will have installed keystone, glance, nova, cinder, neutron, and horizon. Floating IPs will be available, guests have access to the external world.

You can access horizon to experience the web interface to OpenStack, and manage vms, networks, volumes, and images from there.

You can source openrc in your shell, and then use the openstack command line tool to manage your devstack.

You can cd /opt/stack/tempest and run tempest tests that have been configured to work with your devstack.

You can make code changes to OpenStack and validate them.