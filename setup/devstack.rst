===========================
Install devstack <extended>
===========================

Linux
-----
read more: https://docs.openstack.org/devstack/latest/

For local virtual machine
-------------------------
Read: `vm.rst` in `setup` directory 


* To use Redis collector:

enable_plugin osprofiler https://git.openstack.org/openstack/osprofiler master
OSPROFILER_COLLECTOR=redis
OSProfiler plugin will install Redis and configure OSProfiler to use Redis driver

* To use specified driver:

enable_plugin osprofiler https://git.openstack.org/openstack/osprofiler master
OSPROFILER_CONNECTION_STRING=<connection string value>
the driver is chosen depending on the value of OSPROFILER_CONNECTION_STRING variable (refer to the next section for details)
