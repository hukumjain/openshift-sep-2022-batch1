# Day1 

## What is a Hypervisor?
- virtualization software
- allows us to run many OS side by side on a laptop/desktop/server
- Example
   - VMWare
     - VMWare vSphere
     - VMWare Workstation ( Windows/Linux/Mac )
     - VMWare Fusion ( Mac )
   - Oracle
     - VirtualBox ( Free - works in Windows/Linux/Mac )
   - Parallels ( Mac OS-X)
   - KVM ( Kernel Virtual Machine ) - opensource and works in Linux
   - Microsoft
      - Hyper-V
- heavy-weight
   because each VM requires dedicated
      - RAM
      - Storage
      - CPU Cores
     
## What is Container Engine?
- application virtualization
- lightweight virtualization technology
- doesn't require
   - dedicated hardware resources like (CPU cores, RAM, Storage )
- one container represents one application
- it is not an Operating System, just an application process
- let's you create and manage containers and container images with user-friendly commands without knowing any low-level container internal details
- container engine internally depends on other tools to manage images and containers
- Docker container engine depends on runC container runtime to manage containers
- Examples of Container Engine
   - Docker
   - Podman
  
## What is Container Runtime?
- is a tool that manages containers ( create, list, delete, etc., )
- Example
    - runC
    - runC Container engine is used by Docker and many other Container Engines

## What is Container Registry?
- is a server that hosts many Container Images
- With the Container images only your containerized application are deployed into a container

## ECS
- AWS Managed Elastic Container Service

# ACS
- Azure Managed Container Service

## What is Container Orchestration Tool/Platform?
- helps us manage containerized applications
- Examples
   - Docker SWARM ( supports only Docker )
   - Google Kubernetes ( supports multiple container runtimes )
   - AWS - EKS (Elastic Kubernetes Service )
   - Azure - AKS ( Azure Kubernetes Service )
   - Red Hat OpenShift ( supports CRI-O/Podman )
   - ROSA - managed Red Hat OpenShift in AWS
   - Azure - managed Red Hat OpenShift 
- supports services and service discovery
    two type
      1. Internal Service
      2. External Service
- supports the below features
   - deploying containerized application within Orchestration platform
   - scale up/down of your applications instances on demand
   - rolling update
      - allows upgrading/downgrading your application version in a live OpenShift cluster without downtime
   - High Availability ( HA )
     - provides an environment that ensure your applications are live 24/7 ( 365 days ) without any downtime
   - in built monitoring tools
       - health check
       - live check
   - self-healing
     - whenever your application stops responding, orchestration platforms can detect that it attempts to repair by rebooting, deleting bad instance of your application with a new working instance of your application
     - 
## Kubernetes
- is an Container Orchestration Platform from Google
- it supports many container runtimes
- initial days it was supporting Docker by default by in latest versions it stopped supported Docker out of the box
- Kubernetes used Dockershim for interacting with Docker Engine/Runtime
- Dockershim is an interface used by Kubernetes to Docker interactions
- Due to security reasons, Docker is deprecated by Kubernetes in latest versions. As part of this decision, Kubernetes removed Dockershim interface.
- In latest versions, Kubernetes interacts with container runtimes via a common interafce called CRI ( Container Runtime Interface ) which is a specifications
- The CRI specifications has to be implemented by the organization that developed the Container Runtime/Engine or by third-party
- Kuberentes built-in Resources
  - Deployment
  - ReplicaSet
  - Pod
  - Job
  - StatefulSet
  - DaemonSet
- Each Kubernetes is managed by a particular Controller
- Controller is a containerized application runs part of Kubernetes
- Controller keeps monitoring for events
   - new Pod/ReplicaSet/Deployment created
   - Pod/ReplicaSet/Deployment edited
   - Pod/ReplicaSet/Deployment deleted
- supports adding Custom Resource and Custom Operators to manage Custom Resources

## OpenShift
- underhood depends on Kubernetes
- Openshift is a Red Hat's distribution of Kubernetes with many additional features
- OpenShift extended Kubernetes by adding many new Custom Resources and Operators
- is an orchestration platform with addition CI/CD functionalities
- OpenShift supports
   - deploying applications from source code ( S2I - Source to Image )
   - deploying applications from Docker Image
   - deploying application using YAML
- has Cluster of nodes
- nodes are servers which can be a virtual machine runnong on-premise, or ec2 runing on AWS/azure or a physical server
- Nodes are of two types
  1. Master Node
  2. Worker Node

## OpenShift/Kubernetes Master Node
- it has Control Plane Components
- Container Runtime/Engine is installed in every Master & Worker Node
- Control Planes only runs in Master Node
- Control Plane Components
- 1. API Server
- 2. etcd
- 3. Scheduler
- 4. Controller Manager(s)
      - Deployment Controller
      - ReplicaSet Controller
      - Replication Controller
      - Endpoint Controller
      - Job Controller, etc.,

### API Server
- Kubernetes/OpenShift Orchestration features are implemented as REST APIs
- all the Kubernetes/OpenShift components only interact with API Server via REST calls
- API Servers interacts with Kubernets/OpenShift components using events
- stores the application and cluster status in a etcd database
- API Server is the only component that is allowed to interact etcd database

### etcd database
- this database stores and retries data as key/value pairs

### Scheduler
- this is responsible to identify a health Kubernetes/OpenShift node where an application Pod can be deployed

### Controller Managers
- a combination of many Kubernetes/OpenShift Controllers
- provides monitoring features
- each controller manages a particular type of Kubernetes/OpenShift Resource
- Example
   - Deployment Controller Manages Deployment
   - ReplicaSet Controller manages ReplicaSet

## OpenShift/Kubernetes Worker Node
- this is where user application Pods are deployed
- Container Runtime will be installed here

## Common components that runs on Master and Worker Node
- kubelet
    - Kubernetes Container Agent
    - this interacts with the Container Runtime installed on the Node
    - responsible for downloading container image and creating the Pods on the node
    - reports Pod status to the API Server on a frequent basis
    - runs as a daemon service
- kubeadm
    - this is a kubernetes administrative tool
    - used to bootstrap(setup) master node
    - used to join worker nodes to the Kubernetes cluster
    - used to unjoin worker/master node from Kubernetes cluster
   
- kubectl
   - a Kubernetes client tool
   - helps us to issue commands to manage and deploy our application
   - is also supported by OpenShift 
   - behind the scene uses REST API calls to API Server to interact with Kubernetes/OpenShift cluster  
  
- oc
  - is an OpenShift client tool
  - works just like kubectl in Kubernetes
  - behind the scene uses REST API calls to API Server to interact with OpenShift cluster

## Openshift Container OS
- RedHat Enterrprise CoreOS is an operating system used by Orchestration Platforms like Red Hat OpenShift or OKD( Opensource OpenShift )
- comes with pre-installed specific version of CRI-O Container Runtime
- comes with pre-installed specific version of Podman Container Engine

## OpenShift Master Node
- has CRI-O Container runtime installed
- has Podman Container engine installed
- has RedHat Enterprise CoreOS as the Operating System

## OpenShift Worker Node
- has CRI-O Container runtime installed
- has Podman Container engine installed
- has either RHEL or RedHat Enterprise CoreOS as the Operating System


## Installing RedHat OpenShift on your laptop/desktop
<pre>
https://developers.redhat.com/products/openshift-local/overview
</pre>

## Creating a new-project
```
oc new-project <your-project-name>
oc new-project jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-project jegan</b>
Already on project "jegan" on server "https://api.ocp.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname
</pre>

## Listing projects
```
oc get projects
```

Expected output
<pre>
(jegan@tektutor.org)$ oc get projects
NAME                                               DISPLAY NAME   STATUS
ansible-automation-platform                                       Active
default                                                           Active
jegan                                                             Active
kube-node-lease                                                   Active
kube-public                                                       Active
kube-system                                                       Active
openshift                                                         Active
openshift-apiserver                                               Active
openshift-apiserver-operator                                      Active
openshift-authentication                                          Active
openshift-authentication-operator                                 Active
openshift-cloud-controller-manager                                Active
openshift-cloud-controller-manager-operator                       Active
openshift-cloud-credential-operator                               Active
openshift-cloud-network-config-controller                         Active
openshift-cluster-csi-drivers                                     Active
openshift-cluster-machine-approver                                Active
openshift-cluster-node-tuning-operator                            Active
openshift-cluster-samples-operator                                Active
openshift-cluster-storage-operator                                Active
openshift-cluster-version                                         Active
openshift-config                                                  Active
openshift-config-managed                                          Active
openshift-config-operator                                         Active
openshift-console                                                 Active
openshift-console-operator                                        Active
openshift-console-user-settings                                   Active
openshift-controller-manager                                      Active
openshift-controller-manager-operator                             Active
openshift-dns                                                     Active
openshift-dns-operator                                            Active
openshift-etcd                                                    Active
openshift-etcd-operator                                           Active
openshift-host-network                                            Active
openshift-image-registry                                          Active
openshift-infra                                                   Active
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
openshift-insights                                                Active
openshift-kni-infra                                               Active
openshift-kube-apiserver                                          Active
openshift-kube-apiserver-operator                                 Active
openshift-kube-controller-manager                                 Active
openshift-kube-controller-manager-operator                        Active
openshift-kube-scheduler                                          Active
openshift-kube-scheduler-operator                                 Active
openshift-kube-storage-version-migrator                           Active
openshift-kube-storage-version-migrator-operator                  Active
openshift-machine-api                                             Active
openshift-machine-config-operator                                 Active
openshift-marketplace                                             Active
openshift-monitoring                                              Active
openshift-multus                                                  Active
openshift-network-diagnostics                                     Active
openshift-network-operator                                        Active
openshift-node                                                    Active
openshift-oauth-apiserver                                         Active
openshift-openstack-infra                                         Active
openshift-operator-lifecycle-manager                              Active
openshift-operators                                               Active
openshift-ovirt-infra                                             Active
openshift-sdn                                                     Active
openshift-service-ca                                              Active
openshift-service-ca-operator                                     Active
openshift-user-workload-monitoring                                Active
openshift-vsphere-infra                                           Active
tektutor                                                          Active
</pre>

## Finding the current active project
```
oc project
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc project</b>
Using project "jegan" on server "https://api.ocp.tektutor.org:6443".
</pre>

## Switching to a project
```
oc project <your-project-name>
oc project jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ oc project jegan
Already on project "jegan" on server "https://api.ocp.tektutor.org:6443".
</pre>



## Deploying nginx web server in our new project
```
oc create deployment nginx --image=bitnami/nginx:latest
```
<pre>
(jegan@tektutor.org)$ <b>oc create deployment nginx --image=bitnami/nginx:latest</b>
deployment.apps/nginx created
</pre>

## Listing the deployments in the current project
```
oc get deployments
oc get deployment
oc get deploy
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get deployments</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           16m
</pre>

## Listing the replicasets in your current project
```
oc get replicasets
oc get replicaset
oc get rs
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get replicasets</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-78644964b4   1         1         1       19m
</pre>

## Listing Pods
```
oc get pods
oc get pod
oc get po
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get pods</b>
NAME                     READY   STATUS    RESTARTS   AGE
nginx-78644964b4-grf8w   1/1     Running   0          19m
</pre>

## Scale up the pod instances to 3 from 1
```
o
```

Expected output
<pre>
(jegan@tektutor.org)$ oc scale deploy/nginx --replicas=3
deployment.apps/nginx scaled
(jegan@tektutor.org)$ oc get pods
NAME                     READY   STATUS              RESTARTS   AGE
nginx-78644964b4-dxx2c   0/1     ContainerCreating   0          1s
nginx-78644964b4-ht472   0/1     ContainerCreating   0          1s
nginx-78644964b4-nffbd   0/1     ContainerCreating   0          1s
(jegan@tektutor.org)$ oc get pods -w
NAME                     READY   STATUS              RESTARTS   AGE
nginx-78644964b4-dxx2c   0/1     ContainerCreating   0          4s
nginx-78644964b4-ht472   0/1     ContainerCreating   0          4s
nginx-78644964b4-nffbd   0/1     ContainerCreating   0          4s
nginx-78644964b4-nffbd   1/1     Running             0          6s
nginx-78644964b4-ht472   1/1     Running             0          16s
nginx-78644964b4-dxx2c   1/1     Running             0          16s
^C(jegan@tektutor.org)$ oc get pods 
NAME                     READY   STATUS    RESTARTS   AGE
nginx-78644964b4-dxx2c   1/1     Running   0          22s
nginx-78644964b4-ht472   1/1     Running   0          22s
nginx-78644964b4-nffbd   1/1     Running   0          22s
</pre>

## Finding the Pod IP and the node in which they are running
```
oc get pods -o wide
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get pods -o wide</b>
NAME                     READY   STATUS    RESTARTS   AGE     IP             NODE                        NOMINATED NODE   READINESS GATES
nginx-78644964b4-dxx2c   1/1     Running   0          6m36s   10.128.2.89    worker-2.ocp.tektutor.org   <none>           <none>
nginx-78644964b4-ht472   1/1     Running   0          6m36s   10.128.1.66    master-2.ocp.tektutor.org   <none>           <none>
nginx-78644964b4-nffbd   1/1     Running   0          6m36s   10.131.1.157   worker-1.ocp.tektutor.org   <none>           <none>
</pre>
