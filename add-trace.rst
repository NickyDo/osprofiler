==================================
Five ways to add a new trace point
==================================
```bash
from osprofiler import profiler

def some_func():
    profiler.start("point_name", {"any_key": "with_any_value"})
    # your code
    profiler.stop({"any_info_about_point": "in_this_dict"})


@profiler.trace("point_name",
                info={"any_info_about_point": "in_this_dict"},
                hide_args=False)
def some_func2(*args, **kwargs):
    # If you need to hide args in profile info, put hide_args=True
    pass

def some_func3():
    with profiler.Trace("point_name",
                        info={"any_key": "with_any_value"}):
        # some code here

@profiler.trace_cls("point_name", info={}, hide_args=False,
                    trace_private=False)
class TracedClass(object):

    def traced_method(self):
        pass

    def _traced_only_if_trace_private_true(self):
         pass

@six.add_metaclass(profiler.TracedMeta)
class RpcManagerClass(object):
    __trace_args__ = {'name': 'rpc',
                      'info': None,
                      'hide_args': False,
                      'trace_private': False}

     def my_method(self, some_args):
         pass

     def my_method2(self, some_arg1, some_arg2, kw=None, kw2=None)
         pass
```