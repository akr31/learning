#  K8s Deployments

Deployments are controllers that augument Pod capabilities, allowing it to self-heal, scale and update.

Deployments are meant to be used with Stateless applications like web servers, and not for use with stateful apps like database!!

There are two main components on a deployment:
- Spec : This is were we describe the desired state of our deployment
- Controller: the controller takes the specs and manages it, its HA and operates in the background in the CP, reconciling observed and desried state

Deployments are defined in the apps/v1 subgroup.  
Also a deployment template would only manage a single pod template but can manage multiple replicas of the same pod template.
Deployments rely heavily on ReplicaSets. ReplicaSets manage pods and bring self healing and scaling capabilities. Deployments manage Replicasets and add rollouts and rollback features.

Hence in the K8s Inception universe we have the following

Containers        ---> Barebones
      |                                           +
   Pods                ----> Co scheduling
      |                                           +
ReplicaSets      -----> Self healing, Scaling
      |                                           +
Deployments   ----> Rollout, Rollbacks


- In k8s world, self-healing : if a pod managed by a deployment fails ; it is replaced
- scalability: deployments have the capability of adding/remving more pods during certain periods of high/low loads 
- Rolling updates and rollbacks:
  - Deployments allow for zero downtime updates for stateless apps which are loosely coupled; backwards and forwards compatible
  -  When you deploy a version, K8s creates relevant pods and a ReplicaSet which manages their observed state
  -  When you deploy a new version, k8s creates a new ReplicaSet and decreaes the number of pods in the 1st RS and increases the number in the new RS; hence doing phased rollout.
  -  The old RS is not deleted but kept, this is to enable rollbacks. k8s deployments give you control over how to perform rollouts and rollbacks.