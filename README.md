# osprofiler

Openstack consists of multiple project, problem is when processing some request such as booting a virtual machine, openstack have to uses multiple services from different project. 

Therefore, something works too slow and complicated to understand what exactly goes wrong and to locate the bottleneck. That why, to resolve this issue, osprofiler which is powerful library (A small distributed tracing library for Openstack service
), that is going to be used by all openstack project and their python clients. It generates 1 trace / request, go through all involved services, and builds a tree of calls; Otherwise, It can generate trace report in: HTML/JSON/DOT.

It can trace and categorize WSGI, RPC, DB calls

## A trace contains:

* Call hierachy
* Time spent in each service/methods
* Projects/services information
* Logging/debugging information


## Why Osprofiler

* We do not want tight coupling with a non-OpenStack solution
* We should be able to enable or disable tracing without restarting the services
* We should be able to enable tracing for specific API request
* Only certain users should be able to enable tracing
* It should be easy to integrate with OpenStack project
* There is no need to include all methods in the trace

## Treetop

* setup
  * [devstack.rst](/setup/devstack.rst)
  * [setup.rst](/setup/setup.rst)
  * [vm.rst](/setup/vm.rst) 
* enable_osProfiler
  * [init-os.rst](/enable_osProfiler/init-os.rst)
  * redis
    * [redis.rst](/enable_osProfiler/redis/redis.rst)
    * [redis-config-os.rst](/enable_osProfiler/redis/redis-config-os.rst)
  * jaeger
    * [jaeger.rst](/enable_osProfiler/jaeger/jaeger.rst)
    * [jaeger-config-os.rst](/enable_osProfiler/jaeger/jaeger-config-os.rst)
* [Interface-result.rst](/interface-result.rst)
* Trace_work
  * [add-trace.rst](/trace_work/add-trace.rst)
  * [trace-word.rst](/trace_work/trace_work.rst)
    


## What points should be traced by default?

* All HTTP calls - helps to get information about: what HTTP requests were done, duration of calls (latency of service), information about projects involved in request.
* All RPC calls - helps to understand duration of parts of request related to different services in one project. This information is essential to understand which service produce the bottleneck.
* All DB API calls - in some cases slow DB query can produce bottleneck. So it’s quite useful to track how much time request spend in DB layer.
* All driver calls - in case of nova, cinder and others we have vendor drivers. Duration
* ALL SQL requests (turned off by default, because it produce a lot of traffic)