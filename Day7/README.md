# Day7

## Lab - Understanding configmap and Secrets 
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day7/configmapANDsecrets

oc apply -f mysql-configmap.yml
oc apply -f mysql-login-credentials.yml
oc apply -f pod.yml
```
The above pod retrieves the database name from configmap as it is a non-sentive data, while it retrieves the mysql username and password from a secret.  The key/value stored in configmap is stored as plain-text, while the key/value stored in a secrets is stored in an encrypted fashion.

The best practice, you shouldn't commit(checkin) the secret yaml file in your version control.


## You may check my medium blog on Tekton with OpenShift
<pre>
https://medium.com/tektutor/openshift-ci-cd-with-tekton-faa88ba45656
</pre>

## TekTon
- is a knative CI/CD Framework that can be deployed and used with Kubernetes/OpenShift
- it is a opensource project
- TekTon CI/CD follows a serverless architecture unlike Jenkins
- RedHat provides support for TekTon
- Tekton can be installed in Kubernetes/OpenShift as Operators from OperatorHub from Webconsole
- Tekton can also installed in Kubernetes/OpenShift from CI using kubectl/oc

## TekTon Commonly used Resources

- Pipeline
- PipelineRun
- Task
- TaskRun

## What is TekTon Pipeline?
- a Tekton Pipeline represents CI/CD Pipeline
- is a combination of many Tasks executed in sequence/parallel or a combination of sequential tasks and parallel tasks depending on our requiremnt
- has one or more Tasks

## What is a TekTon Task?
- TekTon task is nothing a Pod in Kubernetes/OpenShift
- TekTon Tasks are further broken down into many Steps

## What is a Tekton Step?
- Each Step is a Container that runs within a Task


