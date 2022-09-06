# OpenShift

## ⛹️‍♂️ Lab - Creating an internal service (ClusterIP)
```
oc expose deploy/nginx --type=ClusterIP --port=8080
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc expose deploy/nginx --type=ClusterIP --port=8080</b>
service/nginx exposed
</pre>


## ⛹️‍♂️ Lab - Listing the clusterip(internal) service
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

## ⛹️‍♂️ Lab - Inspecting pod endpoints loadbalanced by service
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

## ⛹️‍♂️ Lab - Creating a public route(url) to access the clusterip service
```
oc expose svc/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ oc expose svc/nginx
route.route.openshift.io/nginx exposed

</pre>

## ⛹️‍♂️ Lab - Listing the routes
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

## ⛹️‍♂️ Lab - Accessing the route
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

## Listing Images in OpenShift private image registry
```
oc get imagestreams
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get imagestreams -n openshift</b>
NAME                                                 IMAGE REPOSITORY                                                                                                TAGS                                                     UPDATED
apicast-gateway                                      image-registry.openshift-image-registry.svc:5000/openshift/apicast-gateway                                      2.1.0.GA,2.10.0.GA,2.11.0.GA,2.11.1.GA + 9 more...       3 weeks ago
apicurito-ui                                         image-registry.openshift-image-registry.svc:5000/openshift/apicurito-ui                                         1.10,1.2,1.3,1.4,1.5,1.6,1.7,1.8                         3 weeks ago
cli                                                  image-registry.openshift-image-registry.svc:5000/openshift/cli                                                  latest                                                   3 weeks ago
cli-artifacts                                        image-registry.openshift-image-registry.svc:5000/openshift/cli-artifacts                                        latest                                                   3 weeks ago
dotnet                                               image-registry.openshift-image-registry.svc:5000/openshift/dotnet                                               3.1,3.1-el7,3.1-ubi8,5.0,5.0-ubi8,6.0 + 2 more...        3 weeks ago
dotnet-runtime                                       image-registry.openshift-image-registry.svc:5000/openshift/dotnet-runtime                                       3.1,3.1-el7,3.1-ubi8,5.0,5.0-ubi8,6.0 + 2 more...        3 weeks ago
driver-toolkit                                       image-registry.openshift-image-registry.svc:5000/openshift/driver-toolkit                                       410.84.202207262020-0,latest                             3 weeks ago
fis-java-openshift                                   image-registry.openshift-image-registry.svc:5000/openshift/fis-java-openshift                                   1.0,2.0                                                  3 weeks ago
fis-karaf-openshift                                  image-registry.openshift-image-registry.svc:5000/openshift/fis-karaf-openshift                                  1.0,2.0                                                  3 weeks ago
fuse-apicurito-generator                             image-registry.openshift-image-registry.svc:5000/openshift/fuse-apicurito-generator                             1.10,1.2,1.3,1.4,1.5,1.6,1.7,1.8                         3 weeks ago
fuse7-console                                        image-registry.openshift-image-registry.svc:5000/openshift/fuse7-console                                        1.0,1.1,1.10,1.2,1.3,1.4,1.5,1.6,1.7 + 1 more...         3 weeks ago
fuse7-eap-openshift                                  image-registry.openshift-image-registry.svc:5000/openshift/fuse7-eap-openshift                                  1.0,1.1,1.10,1.2,1.3,1.4,1.5,1.6,1.7 + 1 more...         3 weeks ago
fuse7-eap-openshift-java11                           image-registry.openshift-image-registry.svc:5000/openshift/fuse7-eap-openshift-java11                           1.10                                                     3 weeks ago
fuse7-java-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/fuse7-java-openshift                                 1.0,1.1,1.10,1.2,1.3,1.4,1.5,1.6,1.7 + 1 more...         3 weeks ago
fuse7-java11-openshift                               image-registry.openshift-image-registry.svc:5000/openshift/fuse7-java11-openshift                               1.10                                                     3 weeks ago
fuse7-karaf-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/fuse7-karaf-openshift                                1.0,1.1,1.10,1.2,1.3,1.4,1.5,1.6,1.7 + 1 more...         3 weeks ago
fuse7-karaf-openshift-jdk11                          image-registry.openshift-image-registry.svc:5000/openshift/fuse7-karaf-openshift-jdk11                          1.10                                                     3 weeks ago
golang                                               image-registry.openshift-image-registry.svc:5000/openshift/golang                                               1.16.7-ubi7,1.16.7-ubi8,latest                           3 weeks ago
httpd                                                image-registry.openshift-image-registry.svc:5000/openshift/httpd                                                2.4,2.4-el7,2.4-el8,latest                               3 weeks ago
installer                                            image-registry.openshift-image-registry.svc:5000/openshift/installer                                            latest                                                   3 weeks ago
installer-artifacts                                  image-registry.openshift-image-registry.svc:5000/openshift/installer-artifacts                                  latest                                                   3 weeks ago
java                                                 image-registry.openshift-image-registry.svc:5000/openshift/java                                                 11,8,latest,openjdk-11-el7,openjdk-11-ubi8 + 3 more...   3 weeks ago
java-runtime                                         image-registry.openshift-image-registry.svc:5000/openshift/java-runtime                                         latest,openjdk-11-ubi8,openjdk-17-ubi8 + 1 more...       3 weeks ago
jboss-amq-62                                         image-registry.openshift-image-registry.svc:5000/openshift/jboss-amq-62                                         1.1,1.2,1.3,1.4,1.5,1.6,1.7                              3 weeks ago
jboss-amq-63                                         image-registry.openshift-image-registry.svc:5000/openshift/jboss-amq-63                                         1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
jboss-datagrid65-client-openshift                    image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid65-client-openshift                    1.0,1.1                                                  3 weeks ago
jboss-datagrid65-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid65-openshift                           1.2,1.3,1.4,1.5,1.6                                      3 weeks ago
jboss-datagrid71-client-openshift                    image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid71-client-openshift                    1.0                                                      3 weeks ago
jboss-datagrid71-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid71-openshift                           1.0,1.1,1.2,1.3                                          3 weeks ago
jboss-datagrid72-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid72-openshift                           1.0,1.1,1.2                                              3 weeks ago
jboss-datagrid73-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid73-openshift                           1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
jboss-datavirt64-driver-openshift                    image-registry.openshift-image-registry.svc:5000/openshift/jboss-datavirt64-driver-openshift                    1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7                          3 weeks ago
jboss-datavirt64-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-datavirt64-openshift                           1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7                          3 weeks ago
jboss-decisionserver64-openshift                     image-registry.openshift-image-registry.svc:5000/openshift/jboss-decisionserver64-openshift                     1.0,1.1,1.2,1.3,1.4,1.5,1.6                              3 weeks ago
jboss-eap-xp3-openjdk11-openshift                    image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap-xp3-openjdk11-openshift                    3.0,latest                                               3 weeks ago
jboss-eap-xp3-openjdk11-runtime-openshift            image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap-xp3-openjdk11-runtime-openshift            3.0,latest                                               3 weeks ago
jboss-eap64-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap64-openshift                                1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8,1.9 + 1 more...          3 weeks ago
jboss-eap70-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap70-openshift                                1.3,1.4,1.5,1.6,1.7                                      3 weeks ago
jboss-eap71-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap71-openshift                                1.1,1.2,1.3,1.4,latest                                   3 weeks ago
jboss-eap72-openjdk11-openshift-rhel8                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap72-openjdk11-openshift-rhel8                1.0,1.1,latest                                           3 weeks ago
jboss-eap72-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap72-openshift                                1.0,1.1,1.2,latest                                       3 weeks ago
jboss-eap73-openjdk11-openshift                      image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-openjdk11-openshift                      7.3,7.3.0,latest                                         3 weeks ago
jboss-eap73-openjdk11-runtime-openshift              image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-openjdk11-runtime-openshift              7.3,7.3.0,latest                                         3 weeks ago
jboss-eap73-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-openshift                                7.3,7.3.0,latest                                         3 weeks ago
jboss-eap73-runtime-openshift                        image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-runtime-openshift                        7.3,7.3.0,latest                                         3 weeks ago
jboss-eap74-openjdk11-openshift                      image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk11-openshift                      7.4.0,latest                                             3 weeks ago
jboss-eap74-openjdk11-runtime-openshift              image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk11-runtime-openshift              7.4.0,latest                                             3 weeks ago
jboss-eap74-openjdk8-openshift                       image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk8-openshift                       7.4.0,latest                                             3 weeks ago
jboss-eap74-openjdk8-runtime-openshift               image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk8-runtime-openshift               7.4.0,latest                                             3 weeks ago
jboss-fuse70-console                                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-console                                 1.0                                                      3 weeks ago
jboss-fuse70-eap-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-eap-openshift                           1.0                                                      3 weeks ago
jboss-fuse70-java-openshift                          image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-java-openshift                          1.0                                                      3 weeks ago
jboss-fuse70-karaf-openshift                         image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-karaf-openshift                         1.0                                                      3 weeks ago
jboss-processserver64-openshift                      image-registry.openshift-image-registry.svc:5000/openshift/jboss-processserver64-openshift                      1.0,1.1,1.2,1.3,1.4,1.5,1.6                              3 weeks ago
jboss-webserver31-tomcat7-openshift                  image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver31-tomcat7-openshift                  1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
jboss-webserver31-tomcat8-openshift                  image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver31-tomcat8-openshift                  1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
jboss-webserver56-openjdk11-tomcat9-openshift-ubi8   image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver56-openjdk11-tomcat9-openshift-ubi8   5.6.0,latest                                             3 weeks ago
jboss-webserver56-openjdk8-tomcat9-openshift-ubi8    image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver56-openjdk8-tomcat9-openshift-ubi8    5.6.0,latest                                             3 weeks ago
jenkins                                              image-registry.openshift-image-registry.svc:5000/openshift/jenkins                                              2,latest                                                 3 weeks ago
jenkins-agent-base                                   image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-base                                   latest                                                   3 weeks ago
jenkins-agent-maven                                  image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-maven                                  latest,v4.0                                              3 weeks ago
jenkins-agent-nodejs                                 image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-nodejs                                 latest,v4.0                                              3 weeks ago
mariadb                                              image-registry.openshift-image-registry.svc:5000/openshift/mariadb                                              10.3,10.3-el7,10.3-el8,10.5-el7 + 2 more...              3 weeks ago
must-gather                                          image-registry.openshift-image-registry.svc:5000/openshift/must-gather                                          latest                                                   3 weeks ago
mysql                                                image-registry.openshift-image-registry.svc:5000/openshift/mysql                                                8.0,8.0-el7,8.0-el8,latest                               3 weeks ago
nginx                                                image-registry.openshift-image-registry.svc:5000/openshift/nginx                                                1.18-ubi7,1.18-ubi8,1.20-ubi7,1.20-ubi8 + 1 more...      3 weeks ago
nodejs                                               image-registry.openshift-image-registry.svc:5000/openshift/nodejs                                               14-ubi7,14-ubi8,14-ubi8-minimal,16-ubi8 + 2 more...      3 weeks ago
oauth-proxy                                          image-registry.openshift-image-registry.svc:5000/openshift/oauth-proxy                                          v4.4                                                     3 weeks ago
openjdk-11-rhel7                                     image-registry.openshift-image-registry.svc:5000/openshift/openjdk-11-rhel7                                     1.0,1.1,1.10                                             3 weeks ago
openjdk-11-rhel8                                     image-registry.openshift-image-registry.svc:5000/openshift/openjdk-11-rhel8                                     1.0                                                      3 weeks ago
perl                                                 image-registry.openshift-image-registry.svc:5000/openshift/perl                                                 5.26-ubi8,5.30,5.30-el7,5.30-ubi8 + 1 more...            3 weeks ago
php                                                  image-registry.openshift-image-registry.svc:5000/openshift/php                                                  7.3,7.3-ubi7,7.4-ubi8,latest                             3 weeks ago
postgresql                                           image-registry.openshift-image-registry.svc:5000/openshift/postgresql                                           10,10-el7,10-el8,12,12-el7,12-el8 + 3 more...            3 weeks ago
python                                               image-registry.openshift-image-registry.svc:5000/openshift/python                                               2.7,2.7-ubi7,2.7-ubi8,3.6-ubi8,3.8 + 4 more...           3 weeks ago
redhat-openjdk18-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/redhat-openjdk18-openshift                           1.0,1.1,1.10,1.2,1.3,1.4,1.5,1.6,1.7 + 1 more...         3 weeks ago
redhat-sso70-openshift                               image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso70-openshift                               1.3,1.4                                                  3 weeks ago
redhat-sso71-openshift                               image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso71-openshift                               1.0,1.1,1.2,1.3                                          3 weeks ago
redhat-sso72-openshift                               image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso72-openshift                               1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
redhat-sso73-openshift                               image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso73-openshift                               1.0,latest                                               3 weeks ago
redis                                                image-registry.openshift-image-registry.svc:5000/openshift/redis                                                5,5-el7,5-el8,6-el7,6-el8,latest                         3 weeks ago
rhdm-decisioncentral-rhel8                           image-registry.openshift-image-registry.svc:5000/openshift/rhdm-decisioncentral-rhel8                           7.11.1                                                   3 weeks ago
rhdm-kieserver-rhel8                                 image-registry.openshift-image-registry.svc:5000/openshift/rhdm-kieserver-rhel8                                 7.11.1                                                   3 weeks ago
rhpam-businesscentral-monitoring-rhel8               image-registry.openshift-image-registry.svc:5000/openshift/rhpam-businesscentral-monitoring-rhel8               7.11.1                                                   3 weeks ago
rhpam-businesscentral-rhel8                          image-registry.openshift-image-registry.svc:5000/openshift/rhpam-businesscentral-rhel8                          7.11.1                                                   3 weeks ago
rhpam-kieserver-rhel8                                image-registry.openshift-image-registry.svc:5000/openshift/rhpam-kieserver-rhel8                                7.11.1                                                   3 weeks ago
rhpam-smartrouter-rhel8                              image-registry.openshift-image-registry.svc:5000/openshift/rhpam-smartrouter-rhel8                              7.11.1                                                   3 weeks ago
ruby                                                 image-registry.openshift-image-registry.svc:5000/openshift/ruby                                                 2.5-ubi8,2.6,2.6-ubi7,2.6-ubi8,2.7 + 5 more...           3 weeks ago
sso74-openshift-rhel8                                image-registry.openshift-image-registry.svc:5000/openshift/sso74-openshift-rhel8                                7.4,latest                                               3 weeks ago
sso75-openshift-rhel8                                image-registry.openshift-image-registry.svc:5000/openshift/sso75-openshift-rhel8                                7.5,latest                                               3 weeks ago
tests                                                image-registry.openshift-image-registry.svc:5000/openshift/tests                                                latest                                                   3 weeks ago
tools                                                image-registry.openshift-image-registry.svc:5000/openshift/tools                                                latest                                                   3 weeks ago
ubi8-openjdk-11                                      image-registry.openshift-image-registry.svc:5000/openshift/ubi8-openjdk-11                                      1.10,1.3                                                 3 weeks ago
ubi8-openjdk-11-runtime                              image-registry.openshift-image-registry.svc:5000/openshift/ubi8-openjdk-11-runtime                              1.10,1.9                                                 3 weeks ago
ubi8-openjdk-8                                       image-registry.openshift-image-registry.svc:5000/openshift/ubi8-openjdk-8                                       1.10,1.3                                                 3 weeks ago
ubi8-openjdk-8-runtime                               image-registry.openshift-image-registry.svc:5000/openshift/ubi8-openjdk-8-runtime                               1.10,1.9                                                 3 weeks ago
</pre>
