# Day3

## ⛹️‍♀️ Lab - Creating a NodePort external service for nginx deployment
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

### Accessing the NodePort service

Find the Node IP Addresses
```
oc get nodes -o wide
```
Expected output
<pre>
(jegan@tektutor.org)$ oc get nodes -o wide
NAME                        STATUS   ROLES           AGE   VERSION           INTERNAL-IP       EXTERNAL-IP   OS-IMAGE                                                        KERNEL-VERSION                 CONTAINER-RUNTIME
master-1.ocp.tektutor.org   Ready    master,worker   26d   v1.23.5+012e945   192.168.122.165   <none>        Red Hat Enterprise Linux CoreOS 410.84.202207262020-0 (Ootpa)   4.18.0-305.57.1.el8_4.x86_64   cri-o://1.23.3-11.rhaos4.10.gitddf4b1a.1.el8
master-2.ocp.tektutor.org   Ready    master,worker   26d   v1.23.5+012e945   192.168.122.250   <none>        Red Hat Enterprise Linux CoreOS 410.84.202207262020-0 (Ootpa)   4.18.0-305.57.1.el8_4.x86_64   cri-o://1.23.3-11.rhaos4.10.gitddf4b1a.1.el8
master-3.ocp.tektutor.org   Ready    master,worker   26d   v1.23.5+012e945   192.168.122.218   <none>        Red Hat Enterprise Linux CoreOS 410.84.202207262020-0 (Ootpa)   4.18.0-305.57.1.el8_4.x86_64   cri-o://1.23.3-11.rhaos4.10.gitddf4b1a.1.el8
worker-1.ocp.tektutor.org   Ready    worker          26d   v1.23.5+012e945   192.168.122.120   <none>        Red Hat Enterprise Linux CoreOS 410.84.202207262020-0 (Ootpa)   4.18.0-305.57.1.el8_4.x86_64   cri-o://1.23.3-11.rhaos4.10.gitddf4b1a.1.el8
worker-2.ocp.tektutor.org   Ready    worker          26d   v1.23.5+012e945   192.168.122.75    <none>        Red Hat Enterprise Linux CoreOS 410.84.202207262020-0 (Ootpa)   4.18.0-305.57.1.el8_4.x86_64   cri-o://1.23.3-11.rhaos4.10.gitddf4b1a.1.el8
</pre>

Let's access the node port external service

```
curl http://<master-1-node-ip>:<node-port>
curl http://<master-2-node-ip>:<node-port>
curl http://<master-3-node-ip>:<node-port>
curl http://<worker-1-node-ip>:<node-port>
curl http://<worker-2-node-ip>:<node-port>

curl http://192.168.122.218:30764
```
