You need to put tracepoint to each function of service to enable OS profiler functionality
* Core function services Nova, keystone, neutron, cinder, glance
eg:+ suppose that we have 3 services A, B, C, service A have function A, B have function B,
C have function C; request come into service A, function A call function B, function B call function C.
each function will have tracepoint 
when the request passes the tracepoint, it calls `notifier` that is defied in OS provider source code
and what notifier does is to store data like timestamp, name of service that cause notifier and so on to the trace database (redis, jaeger tracing, mongoDB, elasticSearch, loginsite)

Every call of profiler.start() & profiler.stop() sends to collector 1 message. It means that every trace point creates 2 records in the collector. 

E.g.: The sample below produces 2 trace points:
```
profiler.start("parent_point")
profiler.start("child_point")
profiler.stop()
profiler.stop()
```
=================================================
Trace points contain 2 messages (start and stop).
=================================================
 Messages like below are sent to a collector:
 ```
 {
    "name": <point_name>-(start|stop)
    "base_id": <uuid>,
    "parent_id": <uuid>,
    "trace_id": <uuid>,
    "info": <dict>
}
```
The fields are defined as the following:

* base_id - <uuid> that is equal for all trace points that belong to one trace, this is done to simplify the process of retrieving all trace points related to one trace from collector
* parent_id - <uuid> of parent trace point
* trace_id - <uuid> of current trace point
* info - the dictionary that contains user information passed when calling profiler start() & stop() methods.

## How to initialize profiler, to get one trace across all services?
To enable cross service profiling we actually need to do send `(base_id & trace_id)` from caller to callee. So callee will be able to init its profiler with these values.

In case of OpenStack, to interact between 2 services, what we need is to:

- Put in python clients headers with trace info (if profiler is inited)

