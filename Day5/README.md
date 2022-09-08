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
Expected output (Only relevant output is captured to avoid bloating with unnecessary logs)
<pre>
(jegan@tektutor.org)$ <b>oc logs -f bc/spring-hello</b>
Cloning "https://github.com/tektutor/openshift-sep-2022-batch1.git" ...
	Commit:	63b8d7517ab2c8a8a80879f90dea7511d1b5359c (Updated buildconfig to point to latest repo.)
	Author:	Jeganathan Swaminathan <mail2jegan@gmail.com>
	Date:	Fri Sep 9 04:10:30 2022 +0530
time="2022-09-08T22:45:49Z" level=info msg="Not using native diff for overlay, this may cause degraded performance for building images: kernel has CONFIG_OVERLAY_FS_REDIRECT_DIR enabled"
I0908 22:45:49.332564       1 defaults.go:102] Defaulting to storage driver "overlay" with options [mountopt=metacopy=on].
Caching blobs under "/var/cache/blobs".

Pulling image registry.redhat.io/ubi8/openjdk-11:latest ...
Trying to pull registry.redhat.io/ubi8/openjdk-11:latest...
Getting image source signatures
Copying blob sha256:f94f36311f7d847df82ebea1c65f0f7ab3479f519a72e6ffef7964c2e0789343
Copying blob sha256:b336a633f9e2c8ec18c45427d7e957df6d20ec09e71eee92c2f3247bb63c2d66
Copying blob sha256:d6e3788a121c267b721a23e030dd339c41f70721e09bdf4b21051cfb22421025
Copying config sha256:2e897fa1efd14a9365bb5a60d681d211bdcfeffe7fb4c0da176872b0bad12caf
Writing manifest to image destination
Storing signatures
Adding transient rw bind mount for /run/secrets/rhsm
[1/2] STEP 1/6: FROM registry.redhat.io/ubi8/openjdk-11:latest AS builder
[1/2] STEP 2/6: MAINTAINER Jeganathan Swaminathan <jegan@tektutor.org>
--> 1b083dffe68
[1/2] STEP 3/6: RUN mkdir -p -m 0700 ./hello/target
time="2022-09-08T22:46:10Z" level=warning msg="Adding metacopy option, configured globally"
--> da1e1e4a96d
[1/2] STEP 4/6: WORKDIR ./hello
--> 975127a41e5
[1/2] STEP 5/6: COPY . ./
time="2022-09-08T22:46:11Z" level=warning msg="Adding metacopy option, configured globally"
--> 69ee4504b91
[1/2] STEP 6/6: RUN mvn package && cp ./target/spring-hello-1.0.jar /tmp/app.jar
[INFO] Scanning for projects...
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  03:06 min
[INFO] Finished at: 2022-09-08T22:49:21Z
[INFO] ------------------------------------------------------------------------
time="2022-09-08T22:49:21Z" level=warning msg="Adding metacopy option, configured globally"
--> 22323284626
[2/2] STEP 1/5: FROM registry.redhat.io/ubi8/openjdk-11:latest AS runner
[2/2] STEP 2/5: COPY --from=builder /tmp/app.jar .
time="2022-09-08T22:49:26Z" level=warning msg="Adding metacopy option, configured globally"
--> 1821f8b6c85
[2/2] STEP 3/5: CMD ["java","-jar","./app.jar"]
--> 22add4d24a3
[2/2] STEP 4/5: ENV "OPENSHIFT_BUILD_NAME"="spring-hello-1" "OPENSHIFT_BUILD_NAMESPACE"="jegan" "OPENSHIFT_BUILD_SOURCE"="https://github.com/tektutor/openshift-sep-2022-batch1.git" "OPENSHIFT_BUILD_COMMIT"="63b8d7517ab2c8a8a80879f90dea7511d1b5359c"
--> b1998e19bb1
[2/2] STEP 5/5: LABEL "io.openshift.build.commit.author"="Jeganathan Swaminathan <mail2jegan@gmail.com>" "io.openshift.build.commit.date"="Fri Sep 9 04:10:30 2022 +0530" "io.openshift.build.commit.id"="63b8d7517ab2c8a8a80879f90dea7511d1b5359c" "io.openshift.build.commit.message"="Updated buildconfig to point to latest repo." "io.openshift.build.commit.ref"="main" "io.openshift.build.name"="spring-hello-1" "io.openshift.build.namespace"="jegan" "io.openshift.build.source-context-dir"="Day5/ImageStreamAndBuildConfig" "io.openshift.build.source-location"="https://github.com/tektutor/openshift-sep-2022-batch1.git"
[2/2] COMMIT temp.builder.openshift.io/jegan/spring-hello-1:14cbaefe
--> 9b5c5a6fed3
Successfully tagged temp.builder.openshift.io/jegan/spring-hello-1:14cbaefe
9b5c5a6fed342006f55f3dd00f38b50bec44b8938816cc59b3adf90f14c200c8

Pushing image image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello:latest ...
Getting image source signatures
Copying blob sha256:b336a633f9e2c8ec18c45427d7e957df6d20ec09e71eee92c2f3247bb63c2d66
Copying blob sha256:d6e3788a121c267b721a23e030dd339c41f70721e09bdf4b21051cfb22421025
Copying blob sha256:f94f36311f7d847df82ebea1c65f0f7ab3479f519a72e6ffef7964c2e0789343
Copying blob sha256:ad3afacdb9c6f1bc5fbe86be1e13eb8e540fa504c2a07df5b7d4c133e848d483
Copying config sha256:9b5c5a6fed342006f55f3dd00f38b50bec44b8938816cc59b3adf90f14c200c8
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello@sha256:8a2ac9c01b3623370471016f6f606b614d884e0de8f9f08ce188eefc762fdcd3
Push successful
</pre>

## ⛹️‍♀️ Lab - Deleting an image from OpenShift container registry
```
oc get images| grep image-registry.openshift-image-registry.svc:5000 | grep tektutor-spring-hello

oc delete image sha256:22f454e8a66f899723017653de4e2d182ef0142ba26ac60d472e48eae7dda58b
```
Expected output
<pre>
oc get images| grep image-registry.openshift-image-registry.svc:5000 | grep tektutor-spring-hello
sha256:684ec0954fe19624b9cbe9bd4ae95d68a8325593cfb78ebedc591cc90eb1f22a   image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello@sha256:684ec0954fe19624b9cbe9bd4ae95d68a8325593cfb78ebedc591cc90eb1f22a
sha256:8a2ac9c01b3623370471016f6f606b614d884e0de8f9f08ce188eefc762fdcd3   image-registry.openshift-image-registry.svc:5000/jegan/tektutor-spring-hello@sha256:8a2ac9c01b3623370471016f6f606b614d884e0de8f9f08ce188eefc762fdcd3

(jegan@tektutor.org)$ oc delete image sha256:684ec0954fe19624b9cbe9bd4ae95d68a8325593cfb78ebedc591cc90eb1f22a
image.image.openshift.io "sha256:684ec0954fe19624b9cbe9bd4ae95d68a8325593cfb78ebedc591cc90eb1f22a" deleted
</pre>

## Deploying application as a template
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day5/templates
oc apply -f nginx-template.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f nginx-template.yml</b>
template.template.openshift.io/tektutor-nginx-template created
</pre>
