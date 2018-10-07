Every call of profiler.start() & profiler.stop() sends to collector 1 message. It means that every trace point creates 2 records in the collector. 

E.g.: The sample below produces 2 trace points:
```bash
profiler.start("parent_point")
profiler.start("child_point")
profiler.stop()
profiler.stop()
```
### Trace points contain 2 messages (start and stop).
 Messages like below are sent to a collector:
 ```bash
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
