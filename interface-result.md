## Get login information

### Step 1
Change to the devstack directory:
```bash
cd devstack/
```

### Step 2
Type `source openrc admin admin` - to get login information with admin rights.
This is needed because the openstack client will use it to log on and run command.
```bash
$ source openrc admin admin
```

## How to get trace
```bash
openstack --os-profile 'hmac_keys' image list
```
- `--os-profile: The option to pass the encoding key set in front, your encryption`

- `'hmac_keys' = encryption key in config services`

To make it easier for end users to work with profiler from CLI, OSProfiler has entry point that allows them to retrieve information about traces and present it in human readable form. In this case, we have:
* Results of profiling can be obtained in JSON (option: --json) and HTML (option: --html) formats:
```bash
$ osprofiler trace show <trace_id> --json/--html
```
* option --out will redirect result of osprofiler trace show in specified file:

```bash
$ osprofiler trace show <trace_id> --json/--html --out /path/to/file
```