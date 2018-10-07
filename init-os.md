OSProfiler now contains various storage drivers to collect tracing data. Information about what driver to use and what options to pass to OSProfiler are now stored in OpenStack services configuration files. Example of such configuration can be found below:

```bash
[profiler]
enabled = True
trace_sqlalchemy = True
hmac_keys = SECRET_KEY
connection_string = messaging://
```

If such configuration is provided, OSProfiler setting up can be processed in following way:

```bash
if CONF.profiler.enabled:
    osprofiler_initializer.init_from_conf(
        conf=CONF,
        context=context.get_admin_context().to_dict(),
        project="cinder",
        service=binary,
        host=host
    )
```