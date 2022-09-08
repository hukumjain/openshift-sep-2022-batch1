# Day 4

## Lab - Deploying application from existing Container Image
```
oc delete project jegan

oc new-project jegan
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --port=8080
oc expose svc/nginx
```

## Lab - Deploying application from Dockerfile from a GitHub Repo
```
oc delete deploy/hello

oc new-app --name=hello https://github.com/tektutor/spring-ms.git
```


## Lab - Deploying application from source code from a GitHub repo
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
