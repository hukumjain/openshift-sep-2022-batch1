# Day 5

## ⛹️‍♀️ Lab - Using multi-stage Dockerfile to build and deploy your application from source code from GitHub repo
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


## ⛹️‍♀️ Lab - Creating a Building with an docker image output

Let's first create the ImageStream resource
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day5/ImageStreamAndBuildConfig
oc apply -f imagestream.yml 
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f imagestream.yml</b>
imagestream.image.openshift.io/tektutor-spring-hello created
</pre>

Listing the imagestream
```
(jegan@tektutor.org)$ <b>oc get is</b>
NAME                    IMAGE REPOSITORY                                                               TAGS   UPDATED
tektutor-spring-hello   image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello 
```

Let's create the buildconfig and start the build
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day5/ImageStreamAndBuildConfig
oc apply -f buildconfig.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ oc apply -f buildconfig.yml 
buildconfig.build.openshift.io/spring-hello created
</pre>

Listing the build config
```
oc get bc
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get bc</b>
NAME           TYPE     FROM   LATEST
spring-hello   Docker   Git    1
</pre>

Checking the build output
```
oc logs -f bc/spring-hello
```

Expected output (Only the last few lines are pasted here)
<pre>
30076a5355466d822b2e3466d9a4c3910ecc"
--> 700719db884
[2/2] STEP 5/5: LABEL "io.openshift.build.commit.author"="Jeganathan Swaminathan <mail2jegan@gmail.com>" "io.openshift.build.commit.date"="Fri Aug 12 06:40:48 2022 +0530" "io.openshift.build.commit.id"="5a2a30076a5355466d822b2e3466d9a4c3910ecc" "io.openshift.build.commit.message"="Update README.md" "io.openshift.build.commit.ref"="main" "io.openshift.build.name"="spring-hello-1" "io.openshift.build.namespace"="jegan" "io.openshift.build.source-context-dir"="Day5/ImageStreamAndBuildConfig" "io.openshift.build.source-location"="https://github.com/tektutor/openshift-aug-2022.git"
[2/2] COMMIT temp.builder.openshift.io/jegan/spring-hello-1:81ec71e7
--> f447bd29660
Successfully tagged temp.builder.openshift.io/jegan/spring-hello-1:81ec71e7
f447bd29660c4b70800af1dc1adc056a5a2073f4896f0ee963ad21bc539c23ed

Pushing image image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello:latest ...
Getting image source signatures
Copying blob sha256:1e09a5ee0038fbe06a18e7f355188bbabc387467144abcd435f7544fef395aa1
Copying blob sha256:0d725b91398ed3db11249808d89e688e62e511bbd4a2e875ed8493ce1febdb2c
Copying blob sha256:e441d34134fac91baa79be3e2bb8fb3dba71ba5c1ea012cb5daeb7180a054687
Copying blob sha256:dab0c9d1d83f19939b2d1c95969e68abd9ba8b4e0f2b6c4866e3355393b4a2ca
Copying config sha256:f447bd29660c4b70800af1dc1adc056a5a2073f4896f0ee963ad21bc539c23ed
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello@sha256:faecafdf6485c222df0b56e97cce4a36cd843f3473f91d987303b58b785ba9ce
Push successful
</pre>
