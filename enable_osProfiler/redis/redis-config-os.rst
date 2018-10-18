Services: Nova, keystone, glance, cinder
===============
Config services
===============

Step 1-location of files config:
--------------------------------

* Nova: /etc/nova/nova.conf
* Keystone: /etc/keystone/keystone.conf <neglected>
* Cinder: /etc/cinder/cinder.conf
* Glance: /etc/glance/glance-api.conf

Step 2-Adding content to files service
--------------------------------------
::
    [profiler]
    enabled = true
    trace_sqlalchemy = true
    connection_string = redis://localhost:6379
    hmac_keys = your encryption


`With keystone: trace_sqlalchemy = false`

Step 3-Restart services
-----------------------
::
    sudo systemctl restart <service-name>


replacing `<service-name>` by the corresponding name:
* Nova: devstack@n-api.service
* Keystone: devstack@keystone.service
* Cinder: devstack@c-api.service
* Glance: devstack@g-api.service
