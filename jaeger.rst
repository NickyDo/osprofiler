============
Start Jaeger
============
We will be using an open source distributed tracing system Jaeger to collect and view the traces and analyze the application’s behavior. Let’s run Jaeger backend as an all-in-one Docker image (see up to date instructions):
```bash
docker run -d -p6831:6831/udp -p16686:16686 jaegertracing/all-in-one:latest
```
Once the container starts, open http://127.0.0.1:16686/ in the browser to access the Jaeger UI.