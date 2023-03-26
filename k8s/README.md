# K8s

K8s is a container orchestration platform. {{ADD MORE GLOBE HERE}}


## The concept of state

A central concept in k8s is the concept of states. For anything defined in k8s there is a 
-  Desired state > The ones which you define in your config
-  Observed state > The state with k8s controllers see deployed in the system

Everyone's happy as long as the desired and observed states match.

But in the case they done match the process of reconcilliation kicks in. This process tries to bring the desired and observed state in sync.

The above approach is critical to a the declarative paradigm of resource management with k8s supports.

- Anything beloe `.spec` in the template relates to the deployment; anything below, `template.spec`  relates to the POD template
- A deployments label selector is immutable, i.e. it cannot be deleted once created!
- It might be imperetive to think that we may control static pods by providing the correct label selector; though this was possible in the early days of k8s, its not supported now as now a pod-template-hash is also used with the app label for the selection!

## Commands

- `kubectl apply -f <file_name>.yml` > Posts the deployment to the API server for K8s to configure
- `kubectl get deploy <deployment>` > Gets the deployment details 
- `kubectl rollout status deployment <deployment_name>` > this command will display the status of rollout of the deployment
- `kubectl rollout pause deployment <deployment_name>` > this command will pause the rollout of the deployment
- `kubectl rollout resume deployment <deployment_name>` > this command will resume the rollout of the deployment
- `kubectl rollout history deployment <deployment_name>` > hisory of all rollouts of a given deployment
- `kubectl rollout undo deployment <deployment_name> --to-revision=<number>` > updates the rollout to a given revision number, used for rollback