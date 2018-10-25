Setup
=====

Install devstack <extended>
---------------------------

Linux
~~~~~
read more: https://docs.openstack.org/devstack/latest/

For local virtual machine
~~~~~~~~~~~~~~~~~~~~~~~~~

* Step 1

Change to the devstack directory:
::
    cd devstack/


* Step 2

Determine your IP address:
::
    sudo ifconfig

You will see a response something like this:
::
    enp0s3 Link encap:Ethernet  HWaddr 08:00:27:ea:97:9f  
        inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
        inet6 addr: fe80::d715:6025:f469:6f7c/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:1364291 errors:0 dropped:0 overruns:0 frame:0
        TX packets:406550 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:1415725472 (1.4 GB)  TX bytes:27942474 (27.9 MB)

    lo     Link encap:Local Loopback  
        inet addr:127.0.0.1  Mask:255.0.0.0
        inet6 addr: ::1/128 Scope:Host
        UP LOOPBACK RUNNING  MTU:65536  Metric:1
        RX packets:229208 errors:0 dropped:0 overruns:0 frame:0
        TX packets:229208 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:89805124 (89.8 MB)  TX bytes:89805124 (89.8 MB)

You’re looking for the addr: value in an interface other than lo.  For example, in this case, it’s 10.0.2.15.  Make note of that for the next step. If you run your VM on the cloud, make sure to use the public IP of your VM in the link.

* Step 3

Create the local.conf file with 4 passwords and your host IP, which you determined in the last step:
::
    cat >  local.conf <<EOF
    [[local|localrc]]
    ADMIN_PASSWORD=secret
    DATABASE_PASSWORD=\$ADMIN_PASSWORD
    RABBIT_PASSWORD=\$ADMIN_PASSWORD
    SERVICE_PASSWORD=\$ADMIN_PASSWORD
    HOST_IP=10.0.2.15
    RECLONE=yes
    EOF

This is the minimum required config to get started with DevStack.


* To use Redis collector:

enable_plugin osprofiler https://git.openstack.org/openstack/osprofiler master
OSPROFILER_COLLECTOR=redis
OSProfiler plugin will install Redis and configure OSProfiler to use Redis driver

* To use specified driver:

enable_plugin osprofiler https://git.openstack.org/openstack/osprofiler master
OSPROFILER_CONNECTION_STRING=<connection string value>
the driver is chosen depending on the value of OSPROFILER_CONNECTION_STRING variable (refer to the next section for details)

Install Osprofiler
------------------ 

Osprofiler can be easily installed by using pip package 
::
    sudo pip install osprofiler



Install OpenStack Client
------------------------

After installing devstack, usually, openstackclient will be installed.
If type 'openstack' not work, it can be installed via pip.
::
    sudo pip install python-openstackclient

