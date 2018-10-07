# osprofiler
Openstack consists of multiple project, problem is when processing some request such as booting a virtual machine, openstack have to uses multiple services from different project. 

Therefore, something works too slow and complicated to understand what exactly goes wrong and to locate the bottleneck. That why, to resolve this issue, osprofiler which is powerful library, that is going to be used by all openstack project and their python clients. It generates 1 trace / request, go through all involved services, and builds a tree of calls 

* [Install Osprofiler](/setup.md)
* [Install devstack](/devstack.md)
* Enable Osprofiler
  * [Background](/init-os.md)
  * Redis
    * [Install redis](/redis.md)
    * [Configure Osprofiler for services](/redis-config-os.md)
  * jaeger
    * [Install jaeger](/jaeger.md)
    * [Configure Osprofiler for services](/jaeger-config-os.md)
* Trace and work
  * Trace
      * [Five ways to add a new trace point](/add-trace.md)
  * work
      * [How does it work](/trace-work.md)