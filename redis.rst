=========================================
Step 1 - Installing and Configuring Redis
=========================================
In order to get the latest version of Redis, we will use apt to install it from the official Ubuntu repositories.

Update your local apt package cache and install Redis by typing:
```bash
$ sudo apt update
$ sudo apt install redis-server
```
This will download and install Redis and its dependencies. Following this, there is one important configuration change to make in the Redis configuration file, which was generated automatically during the installation.

Open this file with your preferred text editor:

```bash
$ sudo nano /etc/redis/redis.conf
```
nside the file, find the supervised directive. This directive allows you to declare an init system to manage Redis as a service, providing you with more control over its operation. The ``supervised`` directive is set to ``no`` by default. Since you are running Ubuntu, which uses the systemd init system, change this to ``systemd``:

```bash
. . .

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd

. . .
```
That’s the only change you need to make to the Redis configuration file at this point, so save and close it when you are finished. Then, restart the Redis service to reflect the changes you made to the configuration file:
```bash
sudo systemctl restart redis.service
```
With that, you’ve installed and configured Redis and it’s running on your machine. Before you begin using it, though, it’s prudent to first check whether Redis is functioning correctly.
======================
Step 2 - Testing Redis
======================
As with any newly-installed software, it’s a good idea to ensure that Redis is functioning as expected before making any further changes to its configuration. We will go over a handful of ways to check that Redis is working correctly in this step.

Start by checking that the Redis service is running:
```bash
sudo systemctl status redis
```
If it is running without any errors, this command will produce output similar to the following:

```bash
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-06-27 18:48:52 UTC; 12s ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 2421 ExecStop=/bin/kill -s TERM $MAINPID (code=exited, status=0/SUCCESS)
  Process: 2424 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 2445 (redis-server)
    Tasks: 4 (limit: 4704)
   CGroup: /system.slice/redis-server.service
           └─2445 /usr/bin/redis-server 127.0.0.1:6379
. . .
```
Here, you can see that Redis is running and is already enabled, meaning that it is set to start up every time the server boots.