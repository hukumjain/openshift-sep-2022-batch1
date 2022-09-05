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
- 


## OpenShift


