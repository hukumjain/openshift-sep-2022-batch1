# Day3

## Creating a NodePort external service for nginx deployment
```
oc expose deploy/nginx --type=NodePort --port=8080
oc get svc
oc describe svc/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc expose deploy/nginx --type=NodePort --port=8080</b>
service/nginx exposed
(jegan@tektutor.org)$ <b>oc get svc</b>
NAME    TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
nginx   NodePort   172.30.219.84   <none>        8080:30764/TCP   3s
(jegan@tektutor.org)$ <b>oc describe svc nginx</b>
Name:                     nginx
Namespace:                jegan
Labels:                   app=nginx
Annotations:              <none>
Selector:                 app=nginx
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       172.30.219.84
IPs:                      172.30.219.84
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30764/TCP
Endpoints:                10.128.0.150:8080,10.128.2.99:8080,10.129.0.156:8080 + 2 more...
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
</pre>

