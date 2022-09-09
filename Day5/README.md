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

## ⛹️‍♀️ Lab - Deploying application as a template

First we need to create the template in our OpenShift cluster
```
oc delete project jegan
oc new-project jegan

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

Let's deploy nginx from our custom template
```
oc new-app tektutor-nginx-template
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-app tektutor-nginx-template</b>
--> Deploying template "jegan/tektutor-nginx-template" to project jegan

     TekTutor nginx deployment
     ---------
     TekTutor nginx deployment 

--> Creating resources ...
    deployment.apps "nginx" created
    service "nginx" created
    route.route.openshift.io "nginx" created
--> Success
    Access your application via route 'nginx-jegan.apps.ocp.tektutor.org' 
    Run 'oc status' to view your app.
</pre>

List and see all the resources created by our template
```
oc get all
```

Expected output
<pre>
(jegan@tektutor.org)$ oc get all
NAME                        READY   STATUS    RESTARTS   AGE
pod/nginx-68d84d67b-sw9wv   1/1     Running   0          116s

NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
service/nginx   ClusterIP   172.30.20.45   <none>        8080/TCP   116s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           116s

NAME                              DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-68d84d67b   1         1         1       116s

NAME                             HOST/PORT                           PATH   SERVICES   PORT   TERMINATION   WILDCARD
route.route.openshift.io/nginx   nginx-jegan.apps.ocp.tektutor.org          nginx      8080                 None
</pre>

# 🦌 Demo - Install Operator SDK to develop your own Custom Operators

## What are Kubernetes/OpenShift Operators?
- is a way to extend a Kubernetes/OpenShift API
- via operator you can add your own Custom Resources in Kubernetes/OpenShift cluster
- to manage the Custom Resources we also need to provide Custom Controllers
- in short Operator is collection of Custom Resources and Custom Controllers that  manage the Custom Resource
- Operator also helps us bundle/package our Openshift applications as Operators
  which can then later be deployed easily on other Openshift clusters

## What is an Operator Hub?
- a web portal that has many ready to deploy operators for various applications
- RedHat market place has many tested and certified operators that you can use within 
  OpenShift.  RedHat supports the certified operators.
- Community market place has opensource ready to deploy operators but for which you may/may not get suport.

## What is an Operator Lifecycle Manager(OLM)?
- Operator Lifecycle Manage(OLM) is an also an Operator which comes pre-installed
- OLM simplifies installation of other Operators
- OLM integrates Operator Hub within OpenShift web console
- OLM enables installation/uninstallation of operators via the exiting oc/kubectl cli tool

## OpenShift Operator SDK 

For detailed official documentation, you may check here 
<pre>
https://docs.openshift.com/container-platform/4.10/operators/operator_sdk/ansible/osdk-ansible-tutorial.html
</pre>

### Installing Go Programming Language

This url has Go version specific binaries https://go.dev/dl
```
wget https://go.dev/dl/go1.18.2.linux-amd64.tar.gz
tar xvfz go1.18.2.linux-amd64.tar.gz
pwd
```

Maksure the go folder path is exported in your ~/.bashrc file as shown below at the at the end of the file
```
export PATH=/home/jegan/operator-sdk/go/bin:$PATH
```

Apply the changes made in .bashrc
```
source ~/.bashrc
```

Check the verion of go
```
go version
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>go version</b>
go version go1.18.2 linux/amd64
</pre>

### Install OpenShift Operator SDK

This url has OpenShift version specific Operator SDK binary https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/operator-sdk

```
wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/operator-sdk/4.10.12/operator-sdk-v1.16.0-ocp-linux-x86_64.tar.gz

tar xvf operator-sdk-v1.16.0-ocp-linux-x86_64.tar.gz

sudo mv ./operator-sdk /usr/local/bin/operator-sdk
```

Let's us verify the version of operator sdk
```
operator-sdk version
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>operator-sdk version</b>
operator-sdk version: "v1.16.0-ocp", commit: "28da5f640cef1a81d91f156b74b50344fbe84121", kubernetes version: "v1.22", go version: "go1.17.5", GOOS: "linux", GOARCH: "amd64"
</pre>

### Install Docker Community Edition
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Start the Docker service
```
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
newgrp docker
```

Check the docker version and see if you can list the images
```
docker version
docker images
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>docker --version</b>
Docker version 20.10.7, build 20.10.7-0ubuntu5~18.04.3

(jegan@tektutor.org)$ <b>docker images</b>
REPOSITORY                                           TAG                           IMAGE ID       CREATED        SIZE
</pre>

### Install Ansible in CentOS 7.x
Make sure you first upgrade your pip
```
python3 -m pip install --upgrade pip
```

Expected output is
<pre>
(jegan@tektutor.org)$ python3 -m pip install --upgrade pip
Collecting pip
  Cache entry deserialization failed, entry ignored
  Downloading https://files.pythonhosted.org/packages/a4/6d/6463d49a933f547439d6b5b98b46af8742cc03ae83543e4d7688c2420f8b/pip-21.3.1-py3-none-any.whl (1.7MB)
    100% |████████████████████████████████| 1.7MB 710kB/s 
Installing collected packages: pip
Successfully installed pip-21.3.1
</pre>

Now proceed with the ansible installation
```
pip3 install ansible
```
<pre>
(jegan@tektutor.org)$ <b>pip3 install ansible</b>
WARNING: pip is being invoked by an old script wrapper. This will fail in a future version of pip.
Please see https://github.com/pypa/pip/issues/5599 for advice on fixing the underlying issue.
To avoid this problem you can invoke Python with '-m pip' instead of running pip directly.
Defaulting to user installation because normal site-packages is not writeable
Collecting ansible
  Using cached ansible-4.10.0.tar.gz (36.8 MB)
  Preparing metadata (setup.py) ... done
Collecting ansible-core~=2.11.7
  Using cached ansible-core-2.11.12.tar.gz (7.1 MB)
  Preparing metadata (setup.py) ... done
Collecting jinja2
  Using cached Jinja2-3.0.3-py3-none-any.whl (133 kB)
Requirement already satisfied: PyYAML in /home/jegan/.local/lib/python3.6/site-packages (from ansible-core~=2.11.7->ansible) (6.0)
Requirement already satisfied: cryptography in /usr/lib/python3/dist-packages (from ansible-core~=2.11.7->ansible) (2.1.4)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/dist-packages (from ansible-core~=2.11.7->ansible) (21.3)
Collecting resolvelib<0.6.0,>=0.5.3
  Downloading resolvelib-0.5.4-py2.py3-none-any.whl (12 kB)
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-2.0.1-cp36-cp36m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (30 kB)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/dist-packages (from packaging->ansible-core~=2.11.7->ansible) (3.0.9)
Building wheels for collected packages: ansible, ansible-core
  Building wheel for ansible (setup.py) ... done
  Created wheel for ansible: filename=ansible-4.10.0-py3-none-any.whl size=60559881 sha256=97c958fe0b983946ca58fc75c87fd70a1fb02ccf474825f95bab58a1961c658c
  Stored in directory: /home/jegan/.cache/pip/wheels/fd/0b/73/1536be1c3fe3e172e003fa05da85642fa29210760ca928348b
  Building wheel for ansible-core (setup.py) ... done
  Created wheel for ansible-core: filename=ansible_core-2.11.12-py3-none-any.whl size=1952093 sha256=444dcc77090dffe25057150b09b82448e1eadb405f99bf58e81a6d35dc34e734
  Stored in directory: /home/jegan/.cache/pip/wheels/de/a2/0a/cfe72f018b6d3845ab54b29259c0ac20eb169d18063770c09e
Successfully built ansible ansible-core
Installing collected packages: MarkupSafe, resolvelib, jinja2, ansible-core, ansible
Successfully installed MarkupSafe-2.0.1 ansible-4.10.0 ansible-core-2.11.12 jinja2-3.0.3 resolvelib-0.5.4
</pre>

Check the version of ansible 
```
ansible --version
```

Expected outputs is
<pre>
(jegan@tektutor.org)$ <b>ansible --version</b>
ansible 2.9.27
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/jegan/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Mar 18 2022, 13:21:42) [GCC 7.5.0]
</pre>

### Install Ansible Runner
For detailed instructions, you may check here https://ansible-runner.readthedocs.io/en/latest/install/
```
pip install ansible-runner
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>pip install ansible-runner</b>
Collecting ansible-runner
  Downloading https://files.pythonhosted.org/packages/f3/7b/b1e7e449959d53e37daeb454d0581ebf2aa3f3f23d51aaaa5c9835f7fefa/ansible-runner-2.2.0.tar.gz (172kB)
    100% |████████████████████████████████| 174kB 2.1MB/s 
Collecting pexpect>=4.5 (from ansible-runner)
  Downloading https://files.pythonhosted.org/packages/39/7b/88dbb785881c28a102619d46423cb853b46dbccc70d3ac362d99773a78ce/pexpect-4.8.0-py2.py3-none-any.whl (59kB)
    100% |████████████████████████████████| 61kB 3.3MB/s 
Collecting packaging (from ansible-runner)
  Downloading https://files.pythonhosted.org/packages/3e/89/7ea760b4daa42653ece2380531c90f64788d979110a2ab51049d92f408af/packaging-20.9-py2.py3-none-any.whl (40kB)
    100% |████████████████████████████████| 40kB 4.1MB/s 
Collecting python-daemon (from ansible-runner)
  Downloading https://files.pythonhosted.org/packages/b1/cc/2ab0d910548de45eaaa50d0372387951d9005c356a44c6858db12dc6b2b7/python_daemon-2.3.0-py2.py3-none-any.whl
Collecting pyyaml (from ansible-runner)
  Downloading https://files.pythonhosted.org/packages/ba/d4/3cf562876e0cda0405e65d351b835077ab13990e5b92912ef2bf1a2280e0/PyYAML-5.4.1-cp27-cp27mu-manylinux1_x86_64.whl (574kB)
    100% |████████████████████████████████| 583kB 1.5MB/s 
Collecting six (from ansible-runner)
  Downloading https://files.pythonhosted.org/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl
Collecting ptyprocess>=0.5 (from pexpect>=4.5->ansible-runner)
  Downloading https://files.pythonhosted.org/packages/22/a6/858897256d0deac81a172289110f31629fc4cee19b6f01283303e18c8db3/ptyprocess-0.7.0-py2.py3-none-any.whl
Collecting pyparsing>=2.0.2 (from packaging->ansible-runner)
  Downloading https://files.pythonhosted.org/packages/8a/bb/488841f56197b13700afd5658fc279a2025a39e22449b7cf29864669b15d/pyparsing-2.4.7-py2.py3-none-any.whl (67kB)
    100% |████████████████████████████████| 71kB 4.2MB/s 
Collecting docutils (from python-daemon->ansible-runner)
  Downloading https://files.pythonhosted.org/packages/8d/14/69b4bad34e3f250afe29a854da03acb6747711f3df06c359fa053fae4e76/docutils-0.18.1-py2.py3-none-any.whl (570kB)
    100% |████████████████████████████████| 573kB 1.4MB/s 
Collecting lockfile>=0.10 (from python-daemon->ansible-runner)
  Downloading https://files.pythonhosted.org/packages/c8/22/9460e311f340cb62d26a38c419b1381b8593b0bb6b5d1f056938b086d362/lockfile-0.12.2-py2.py3-none-any.whl
Collecting setuptools (from python-daemon->ansible-runner)
  Downloading https://files.pythonhosted.org/packages/e1/b7/182161210a13158cd3ccc41ee19aadef54496b74f2817cc147006ec932b4/setuptools-44.1.1-py2.py3-none-any.whl (583kB)
    100% |████████████████████████████████| 583kB 1.5MB/s 
Building wheels for collected packages: ansible-runner
  Running setup.py bdist_wheel for ansible-runner ... done
  Stored in directory: /home/jegan/.cache/pip/wheels/a4/02/ec/167e79b59e3a3654fcfa44bc3d198d6ac53e87f86c053cdecd
Successfully built ansible-runner
Installing collected packages: ptyprocess, pexpect, pyparsing, packaging, docutils, lockfile, setuptools, python-daemon, pyyaml, six, ansible-runner
Successfully installed ansible-runner-2.2.0 docutils-0.18.1 lockfile-0.12.2 packaging-20.9 pexpect-4.8.0 ptyprocess-0.7.0 pyparsing-2.4.7 python-daemon-2.3.0 pyyaml-5.4.1 setuptools-44.1.1 six-1.16.0
</pre>

### Installing Ansible Http Event Emitter
```
pip install ansible-runner-http
```

### Installing OpenShift python client
```
pip install openshift
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>pip install openshift</b>
Collecting openshift
  Downloading https://files.pythonhosted.org/packages/97/c0/d8e2aae7b4e8f3709eca4fd8c2f70ea3c66151d1a5259e9a7e1ee2497608/openshift-0.13.1.tar.gz
Collecting kubernetes>=12.0 (from openshift)
  Downloading https://files.pythonhosted.org/packages/19/4a/39b09950b35a36fe1af54bb85413c2976b62a5c29e7254c8c3573f86c028/kubernetes-18.20.0-py2.py3-none-any.whl (1.6MB)
    100% |████████████████████████████████| 1.6MB 653kB/s 
Collecting python-string-utils (from openshift)
  Downloading https://files.pythonhosted.org/packages/5d/13/216f2d4a71307f5a4e5782f1f59e6e8e5d6d6c00eaadf9f92aeccfbb900c/python-string-utils-0.6.0.tar.gz
Collecting six (from openshift)
  Using cached https://files.pythonhosted.org/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl
Collecting setuptools>=21.0.0 (from kubernetes>=12.0->openshift)
  Using cached https://files.pythonhosted.org/packages/e1/b7/182161210a13158cd3ccc41ee19aadef54496b74f2817cc147006ec932b4/setuptools-44.1.1-py2.py3-none-any.whl
Collecting ipaddress>=1.0.17; python_version == "2.7" (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/c2/f8/49697181b1651d8347d24c095ce46c7346c37335ddc7d255833e7cde674d/ipaddress-1.0.23-py2.py3-none-any.whl
Collecting requests (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/2d/61/08076519c80041bc0ffa1a8af0cbd3bf3e2b62af10435d269a9d0f40564d/requests-2.27.1-py2.py3-none-any.whl (63kB)
    100% |████████████████████████████████| 71kB 4.4MB/s 
Collecting python-dateutil>=2.5.3 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/36/7a/87837f39d0296e723bb9b62bbb257d0355c7f6128853c78955f57342a56d/python_dateutil-2.8.2-py2.py3-none-any.whl (247kB)
    100% |████████████████████████████████| 256kB 2.6MB/s 
Collecting requests-oauthlib (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/6f/bb/5deac77a9af870143c684ab46a7934038a53eb4aa975bc0687ed6ca2c610/requests_oauthlib-1.3.1-py2.py3-none-any.whl
Collecting websocket-client!=0.40.0,!=0.41.*,!=0.42.*,>=0.32.0 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/f7/0c/d52a2a63512a613817846d430d16a8fbe5ea56dd889e89c68facf6b91cb6/websocket_client-0.59.0-py2.py3-none-any.whl (67kB)
    100% |████████████████████████████████| 71kB 5.5MB/s 
Collecting certifi>=14.05.14 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/37/45/946c02767aabb873146011e665728b680884cd8fe70dde973c640e45b775/certifi-2021.10.8-py2.py3-none-any.whl (149kB)
    100% |████████████████████████████████| 153kB 3.3MB/s 
Collecting urllib3>=1.24.2 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/ec/03/062e6444ce4baf1eac17a6a0ebfe36bb1ad05e1df0e20b110de59c278498/urllib3-1.26.9-py2.py3-none-any.whl (138kB)
    100% |████████████████████████████████| 143kB 3.7MB/s 
Collecting google-auth>=1.0.1 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/fc/04/ea9a945a6fbd7f7f977fcf7e300a715f1635939e5daf141b95068abaa5ec/google_auth-2.6.6-py2.py3-none-any.whl (156kB)
    100% |████████████████████████████████| 163kB 3.3MB/s 
Collecting pyyaml>=5.4.1 (from kubernetes>=12.0->openshift)
  Using cached https://files.pythonhosted.org/packages/ba/d4/3cf562876e0cda0405e65d351b835077ab13990e5b92912ef2bf1a2280e0/PyYAML-5.4.1-cp27-cp27mu-manylinux1_x86_64.whl
Collecting idna<3,>=2.5; python_version < "3" (from requests->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/a2/38/928ddce2273eaa564f6f50de919327bf3a00f091b5baba8dfa9460f3a8a8/idna-2.10-py2.py3-none-any.whl (58kB)
    100% |████████████████████████████████| 61kB 4.3MB/s 
Collecting chardet<5,>=3.0.2; python_version < "3" (from requests->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/19/c7/fa589626997dd07bd87d9269342ccb74b1720384a4d739a1872bd84fbe68/chardet-4.0.0-py2.py3-none-any.whl (178kB)
    100% |████████████████████████████████| 184kB 2.9MB/s 
Collecting oauthlib>=3.0.0 (from requests-oauthlib->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/05/57/ce2e7a8fa7c0afb54a0581b14a65b56e62b5759dbc98e80627142b8a3704/oauthlib-3.1.0-py2.py3-none-any.whl (147kB)
    100% |████████████████████████████████| 153kB 3.6MB/s 
Collecting pyasn1-modules>=0.2.1 (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/95/de/214830a981892a3e286c3794f41ae67a4495df1108c3da8a9f62159b9a9d/pyasn1_modules-0.2.8-py2.py3-none-any.whl (155kB)
    100% |████████████████████████████████| 163kB 3.9MB/s 
Collecting cachetools<6.0,>=2.0.0 (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/2f/a6/30b0a0bef12283e83e58c1d6e7b5aabc7acfc4110df81a4471655d33e704/cachetools-3.1.1-py2.py3-none-any.whl
Collecting rsa<4.6; python_version < "3.6" (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/26/f8/8127fdda0294f044121d20aac7785feb810e159098447967a6103dedfb96/rsa-4.5-py2.py3-none-any.whl
Collecting enum34>=1.1.10; python_version < "3.4" (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/6f/2c/a9386903ece2ea85e9807e0e062174dc26fdce8b05f216d00491be29fad5/enum34-1.1.10-py2-none-any.whl
Collecting pyasn1<0.5.0,>=0.4.6 (from pyasn1-modules>=0.2.1->google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/62/1e/a94a8d635fa3ce4cfc7f506003548d0a2447ae76fd5ca53932970fe3053f/pyasn1-0.4.8-py2.py3-none-any.whl (77kB)
    100% |████████████████████████████████| 81kB 5.0MB/s 
Building wheels for collected packages: openshift, python-string-utils
  Running setup.py bdist_wheel for openshift ... done
  Stored in directory: /home/jegan/.cache/pip/wheels/31/5c/3f/c655e9f2030f180a983293feeec85ff95d2546ae7d3da6cb41
  Running setup.py bdist_wheel for python-string-utils ... done
  Stored in directory: /home/jegan/.cache/pip/wheels/b2/1d/3f/2f554103ba3e4aa8d53b8e1ea70b5a0b4f74409a789e17fa35
Successfully built openshift python-string-utils
Installing collected packages: setuptools, ipaddress, idna, certifi, urllib3, chardet, requests, six, python-dateutil, oauthlib, requests-oauthlib, websocket-client, pyasn1, pyasn1-modules, cachetools, rsa, enum34, google-auth, pyyaml, kubernetes, python-string-utils, openshift
Successfully installed cachetools-3.1.1 certifi-2021.10.8 chardet-4.0.0 enum34-1.1.10 google-auth-2.6.6 idna-2.10 ipaddress-1.0.23 kubernetes-18.20.0 oauthlib-3.1.0 openshift-0.13.1 pyasn1-0.4.8 pyasn1-modules-0.2.8 python-dateutil-2.8.2 python-string-utils-0.6.0 pyyaml-5.4.1 requests-2.27.1 requests-oauthlib-1.3.1 rsa-4.5 setuptools-44.1.1 six-1.16.0 urllib3-1.26.9 websocket-client-0.59.0
</pre>
