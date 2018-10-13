================
Install devstack <extended>
================

Linux
-----
Add Stack User
~~~~~~~~~~~~~~
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
Download DevStack
~~~~~~~~~~~~~~~~~
```bash
$ git clone https://git.openstack.org/openstack-dev/devstack
$ cd devstack
```
The devstack repo contains a script that installs OpenStack and templates for configuration files

Create a local.conf
~~~~~~~~~~~~~~~~~~~
* Create a local.conf file with 4 passwords preset at the root of the devstack git repo.
```bash
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```
* For local virtual machine]
.. _For local virtual machine: /vm.md

* To use Redis collector:

enable_plugin osprofiler https://git.openstack.org/openstack/osprofiler master
OSPROFILER_COLLECTOR=redis
OSProfiler plugin will install Redis and configure OSProfiler to use Redis driver

* To use specified driver:

enable_plugin osprofiler https://git.openstack.org/openstack/osprofiler master
OSPROFILER_CONNECTION_STRING=<connection string value>
the driver is chosen depending on the value of OSPROFILER_CONNECTION_STRING variable (refer to the next section for details)

Start the install
~~~~~~~~~~~~~~~~~
```bash
./stack.sh
```
This will take a 15 - 20 minutes, largely depending on the speed of your internet connection. Many git trees and packages will be installed during this process.

