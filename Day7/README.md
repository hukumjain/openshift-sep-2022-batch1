# Day7

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


