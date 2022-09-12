# Day 6

## OpenShift Operators
- automating the human operators specific skill within OpenShift cluster
- Operators = Custom Resource(s) + Custom Controller(s)
- One Controller manages one type of Resource
- For example:
     - Deployment Controller manages Deployment
     - ReplicaSet Controller manages ReplicaSet
     
## How Custom Resource(CR) are added to OpenShift cluster?
- We need to define Custom Resource Definition(CRD)
- CRDs introduce/register a new type of Custom Resource(CR) to your OpenShift cluster
- CRDs themselves doesn't add a Custom Resource(CR), we need to create them ourselves

## What is an OpenShift Controller?
- it is an application that runs within the OpenShift cluster that monitors a specific type of Resource
- whenever it detects a Resource managed by the Controller is added/edited/deleted/updated, it acts
- Controller does 3 things
    1. monitors a specific Resource (Observe/Watch)
    2. compares desired vs current status
    3. Whenever there is a deviation of current status from desired state, Controllers act to ensure Current state matches the actual State
    
  Example:
    - ReplicaSet Controller monitors the ReplicaSet resources constantly
    - ReplicaSet Controller registers with the APIServer ( informers )
     for CRUD ( Create, Read, Update and Delete ) events of ReplicaSet
    - ReplicaSet Controller will act whenever it sees the Desired Pods doesn't match the current no of Pods
    
   
