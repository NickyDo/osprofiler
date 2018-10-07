Services: Nova, keystone, glance, cinder
## Config services
### Step 1-location of files config:
- Nova: /etc/nova/nova.conf
- Keystone: /etc/keystone/keystone.conf
- Cinder: /etc/cinder/cinder.conf
- Glance: /etc/glance/glance-api.conf
### Step 2-Adding content to files service
```ini
[profiler]
enabled = true
trace_sqlalchemy = true
connection_string = jaeger://localhost:6831
hmac_keys = 123
```
`With keystone: trace_sqlalchemy = false`
### Step 3-Restart services
```bash
sudo systemctl restart <service-name>
```
replacing `<service-name>` by the corresponding name:
- Nova: devstack@n-api.service
- Keystone: devstack@keystone.service
- Cinder: devstack@c-api.service
- Glance: devstack@g-api.service