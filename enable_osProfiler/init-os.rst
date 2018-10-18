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
