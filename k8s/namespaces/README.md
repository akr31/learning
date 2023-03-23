# K8s Namespaces

Namespaces partition the k8s cluster and are designed to easily apply  quotas anda policies to group of objects.

- Most k8s object gets deployed to a namespace(aka namespaced). If one is not specified explicitly an implicit ns `default`  is used
- Default NS'
    - default: newly created objects go here by default; can be overriden from config; unless specified explicitly 
    - kube-system: DNS, metrics and control plane components are deployed here
    - kube-public: objects which need to be readable by anyone
    - kube-node-lease: use for node heartbeat and node lease mgmt
- Add `namespace: <name>` to the metadata section of your object config to ensure it gets deployed to the correct namespace


Commands:
- `kubectl get namespaces` : lists all namespaces
- `kubectl describe ns <name>`: describes the name in a human readable format
- `kubectl create ns <name>`:  creates a new namespace imperetively; not recommended to use
- `kubectl delete ns <name>`: deletes a namespace imperetively; use `-f <file_name>` which is a more declarative method
- `kubectl config set-context --current --namespace <name>` : sets default namespace in the current kubectl context