Enable osProfiler
=================

Background
----------
OSProfiler can send trace data into different collectors which're controled by 2 params:
.# `OSPROFILER_COLLECTOR` specifies which collector to install in Devstack. OSProfiler plugin does not install anything by dafault,
thus default messaging driver will be used

Possible value:

* `<emty> ` - default messaging driver is used
* `redis` - Redis is installed
The default value of OSPROFILER_CONNECTION_STRING is set automatically depending on OSPROFILER_COLLECTOR value.

.# `OSPROFILER_CONNECTION_STRING` specifies which driver is used by OSProfiler.

Possible values:

* messaging:// - use messaging as trace collector (with the transport configured by oslo.messaging)
* redis://[:password]@host[:port][/db] - use Redis as trace storage
* jaeger://[:password]@host[:port][/db] - use jaeger as trace storage
* elasticsearch://host:port - use Elasticsearch as trace storage
* mongodb://host:port - use MongoDB as trace storage
* loginsight://username:password@host - use LogInsight as trace collector/storage

OSProfiler now contains various storage drivers to collect tracing data. Information about what driver to use and what options to pass to OSProfiler are now stored in OpenStack services configuration files. Example of such configuration can be found below:
::
    [profiler]
    enabled = True
    trace_sqlalchemy = True
    hmac_keys = SECRET_KEY
    connection_string = messaging://


If such configuration is provided, OSProfiler setting up can be processed in following way:
::
    if CONF.profiler.enabled:
        osprofiler_initializer.init_from_conf(
            conf=CONF,
            context=context.get_admin_context().to_dict(),
            project="cinder",
            service=binary,
            host=host
        )
Redis
-----

Why Redis
~~~~~~~~~
* Fast
* Non-persistent
* Automatic garbage collection ASAP using Redis built-in TTL
* Memory-efficient
Read more: https://redislabs.com/why-redis/

Setup
~~~~~
Read more: https://redis.io/topics/quickstart




Config services for redis
-------------------------

* Services: Nova, keystone, glance, cinder

Step 1-location of files config:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Nova: /etc/nova/nova.conf
* Keystone: /etc/keystone/keystone.conf <neglected>
* Cinder: /etc/cinder/cinder.conf
* Glance: /etc/glance/glance-api.conf

Step 2-Adding content to files service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Insert:
::
    [profiler]
    enabled = true
    trace_sqlalchemy = true
    connection_string = redis://localhost:6379
    hmac_keys = your encryption


`With keystone: trace_sqlalchemy = false`

Step 3-Restart services
~~~~~~~~~~~~~~~~~~~~~~~
Command:
::
    sudo systemctl restart <service-name>


replacing `<service-name>` by the corresponding name:
* Nova: devstack@n-api.service
* Keystone: devstack@keystone.service <neglected>
* Cinder: devstack@c-api.service
* Glance: devstack@g-api.service

Jaeger
------

Why jaeger
~~~~~~~~~~
* High Scalability
* Native support for OpenTracing
* Multiple storage backends
* Modern Web UI
* Cloud Native Deployment
* Observability
* Backwards compatibility with Zipkin
- More detail: read `https://github.com/jaegertracing/jaeger`


Start Jaeger
~~~~~~~~~~~~
We will be using an open source distributed tracing system Jaeger to collect and view the traces and analyze the application’s behavior. Let’s run Jaeger backend as an all-in-one Docker image (see up to date instructions):
::
    docker run -d -p6831:6831/udp -p16686:16686 jaegertracing/all-in-one:latest

Once the container starts, open http://127.0.0.1:16686/ in the browser to access the Jaeger UI.

Install Jaeger-client
~~~~~~~~~~~~~~~~~~~~~

Command:
::
    sudo pip install jaeger-client


Config services for jaeger
--------------------------

* Services: Nova, keystone, glance, cinder

Step 1-location of files config:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Nova: /etc/nova/nova.conf
- Keystone: /etc/keystone/keystone.conf <neglected>
- Cinder: /etc/cinder/cinder.conf
- Glance: /etc/glance/glance-api.conf

Step 2-Adding content to files service:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Insert:
::
    [profiler]
    enabled = true
    trace_sqlalchemy = true
    connection_string = jaeger://localhost:6831
    hmac_keys = your encryption

`With keystone: trace_sqlalchemy = false`

Step 3-Restart services
~~~~~~~~~~~~~~~~~~~~~~~
Command:
::
    sudo systemctl restart <service-name>

replacing `<service-name>` by the corresponding name:
* Nova: devstack@n-api.service
* Keystone: devstack@keystone.service
* Cinder: devstack@c-api.service
* Glance: devstack@g-api.service
