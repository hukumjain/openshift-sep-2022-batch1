# Day 5

## Using multi-stage Dockerfile to build and deploy your application from source code from GitHub repo
```
oc new-app --name hello https://github.com/tektutor/openshift-sep-2022-batch1.git#main --context-dir=Day5/multi-stage-dockerfile --strategy=docker
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-app --name hello https://github.com/tektutor/openshift-sep-2022-batch1.git#main --context-dir=Day5/multi-stage-dockerfile --strategy=docker</b>
--> Found container image 2e897fa (10 days old) from registry.redhat.io for "registry.redhat.io/ubi8/openjdk-11:latest"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * An image stream tag will be created as "openjdk-11:latest" that will track the source image
    * A Docker build using source code from https://github.com/tektutor/openshift-sep-2022-batch1.git#main will be created
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
</pre>
