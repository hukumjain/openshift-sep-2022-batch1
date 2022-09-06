# OpenShift

## Creating an internal service (ClusterIP)
```
oc expose deploy/nginx --type=ClusterIP --port=8080
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc expose deploy/nginx --type=ClusterIP --port=8080</b>
service/nginx exposed
</pre>
