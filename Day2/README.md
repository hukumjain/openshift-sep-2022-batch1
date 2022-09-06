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


## Listing the clusterip(internal) service
```
oc get services
oc get service
oc get svc
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get services</b>
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.139.58   <none>        8080/TCP   15m
</pre>

## Inspecting pod endpoints loadbalanced by service
```
oc describe svc/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ oc describe svc nginx
Name:              nginx
Namespace:         jegan
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.139.58
IPs:               172.30.139.58
Port:              <unset>  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.128.0.67:8080,10.128.2.98:8080,10.131.1.248:8080
Session Affinity:  None
Events:            <none>
</pre>

## Creating a public route(url) to access the clusterip service
```
oc expose svc/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ oc expose svc/nginx
route.route.openshift.io/nginx exposed

</pre>

## Listing the routes
```
oc get routes
oc get route
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get route</b>
NAME    HOST/PORT                           PATH   SERVICES   PORT   TERMINATION   WILDCARD
nginx   nginx-jegan.apps.ocp.tektutor.org          nginx      8080                 None
</pre>

## Accessing the route
```
curl http://nginx-jegan.apps.ocp.tektutor.org:80
curl http://nginx-jegan.apps.ocp.tektutor.org
curl nginx-jegan.apps.ocp.tektutor.org:80
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>curl nginx-jegan.apps.ocp.tektutor.org</b>
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
</pre>
