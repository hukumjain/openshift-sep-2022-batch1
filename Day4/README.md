# Day 4

## ⛹️‍♂️ Lab - Deploying application from existing Container Image
```
oc delete project jegan

oc new-project jegan
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --port=8080
oc expose svc/nginx
```

## ⛹️‍♀️ Lab - Deploying application from Dockerfile from a GitHub Repo
```
oc delete deploy/hello

oc new-app --name=hello https://github.com/tektutor/spring-ms.git
```


## ⛹️ Lab - Deploying application from source code from a GitHub repo
```
oc delete deploy/hello

oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git</b>
--> Found container image 2e897fa (9 days old) from registry.redhat.io for "registry.redhat.io/ubi8/openjdk-11:latest"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * An image stream tag will be created as "openjdk-11:latest" that will track the source image
    * A source build using source code from https://github.com/tektutor/hello-spring-boot.git will be created
      * The resulting image will be pushed to image stream tag "hello:latest"
      * Every time "openjdk-11:latest" changes a new build will be triggered

--> Creating resources ...
    imagestream.image.openshift.io "openjdk-11" created
    imagestream.image.openshift.io "hello" created
    buildconfig.build.openshift.io "hello" created
    deployment.apps "hello" created
    service "hello" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/hello' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/hello' 
    Run 'oc status' to view your app.
(jegan@tektutor.org)$ <b>oc logs -f buildconfig/hello</b>
</pre>

## Deploying mysql database 
```
oc create deployment mysql --image=bitnami/mysql:latest
oc set env deploy/mysql MYSQL_ROOT_PASSWORD=root
```

Expected output
<pre>
(jegan@tektutor.org)$ oc create deployment mysql --image=bitnami/mysql:latest
deployment.apps/mysql created
(jegan@tektutor.org)$ oc get deploy,po
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql   0/1     1            0           12s

NAME                         READY   STATUS              RESTARTS   AGE
pod/mysql-68bc9bf65d-68c76   0/1     ContainerCreating   0          12s
(jegan@tektutor.org)$ oc get po -w
NAME                     READY   STATUS              RESTARTS   AGE
mysql-68bc9bf65d-68c76   0/1     ContainerCreating   0          20s
mysql-68bc9bf65d-68c76   0/1     Error               0          27s
^C(jegan@tektutor.org)$ oc logs mysql-68bc9bf65d-68c76
mysql 05:10:06.25 
mysql 05:10:06.25 Welcome to the Bitnami mysql container
mysql 05:10:06.26 Subscribe to project updates by watching https://github.com/bitnami/containers
mysql 05:10:06.26 Submit issues and feature requests at https://github.com/bitnami/containers/issues
mysql 05:10:06.26 
mysql 05:10:06.27 INFO  ==> ** Starting MySQL setup **
mysql 05:10:06.30 INFO  ==> Validating settings in MYSQL_*/MARIADB_* env vars
mysql 05:10:06.31 ERROR ==> The MYSQL_ROOT_PASSWORD environment variable is empty or not set. Set the environment variable ALLOW_EMPTY_PASSWORD=yes to allow the container to be started with blank passwords. This is recommended only for development.
(jegan@tektutor.org)$ oc set env deploy/mysql MYSQL_ROOT_PASSWORD=root
deployment.apps/mysql updated
(jegan@tektutor.org)$ oc get po -w
NAME                     READY   STATUS              RESTARTS      AGE
mysql-68bc9bf65d-68c76   0/1     Error               3 (37s ago)   89s
mysql-98798fb95-bgcph    0/1     ContainerCreating   0             3s
mysql-98798fb95-bgcph    1/1     Running             0             6s
mysql-68bc9bf65d-68c76   0/1     Terminating         3 (40s ago)   92s
mysql-68bc9bf65d-68c76   0/1     Terminating         3             92s
mysql-68bc9bf65d-68c76   0/1     Terminating         3             93s
mysql-68bc9bf65d-68c76   0/1     Terminating         3             93s
</pre>
