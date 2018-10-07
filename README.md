# osprofiler
Openstack consists of multiple project, problem is when processing some request such as booting a virtual machine, openstack have to uses multiple services from different project. 

Therefore, something works too slow and complicated to understand what exactly goes wrong and to locate the bottleneck. That why, to resolve this issue, osprofiler which is powerful library, that is going to be used by all openstack project and their python clients. It generates 1 trace / request, go through all involved services, and builds a tree of calls 

* [Install devstack](/devstack.md)
* [Install Osprofiler](/setup.md)
* Enable Osprofiler
  * [Background](/init-os.md)
  * Redis
    * [Install redis](/redis.md)
    * [Configure Osprofiler for services](/redis-config-os.md)
  * jaeger
    * [Install jaeger](/jaeger.md)
    * [Configure Osprofiler for services](/jaeger-config-os.md)
* [Interface result](/interface-result.md)
* Trace and work
  * Trace
      * [Five ways to add a new trace point](/add-trace.md)
  * work
      * [How does it work](/trace-work.md)

## What points should be tracked by default?
- All HTTP calls - helps to get information about: what HTTP requests were done, duration of calls (latency of service), information about projects involved in request.
- All RPC calls - helps to understand duration of parts of request related to different services in one project. This information is essential to understand which service produce the bottleneck.
- All DB API calls - in some cases slow DB query can produce bottleneck. So it’s quite useful to track how much time request spend in DB layer.
- All driver calls - in case of nova, cinder and others we have vendor drivers. Duration
- ALL SQL requests (turned off by default, because it produce a lot of traffic)