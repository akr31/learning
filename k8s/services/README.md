# K8s Services


Services provide a stable and reliable networking interface for a set of Pods.

- When pods fail/(re)deployed, they get replaced, hence the new Pod has a different unique IP address. All these operations created a huge IP churn in the k8s environment. Thus we should never connect to k8s Pods n/w iface directly.
- Each service object gets its own IP address, DNS name and port, each of which are stable (not ephemeral like the Pods)
- Services uses labels and selectors(loose coupling) to determine where to forward the traffic to. Svc's also load balance the traffic to our pods for free!
- Services use Endpoints objects to update the list of healthy matching pods dynamically. Basically Service monitors all healthy pods against the label selector; and if any new pod matches it gets added to Endpoints object; if any pods becomes unhealthy it gets removed
- K8s 1.23+ supports dual stack networking; i.e. all pods and services in k8s have both IPv4 and v6 addresses. We should really use this feature!!


## Types of services:
  
- ClusterIP: it has a stable virtual IP that is only accessible from inside and the cluster. Pods use internal DNS to resolve the ClusterIP of a service from its name. Then send requests to this IP. Routes are distributed on each pod under this sevice, hence the requests get load balanced to one of the pods behind the svc.
- NodePort: build on top of ClusterIP but allow external access by opening up a dedicated port on every cluster node to reach the service. So flow is: externalUser >> svc:NodePort >> nodeA:NodePort >> svc >> endpointResolver >> nodeB:InternalPort
- LoadBalancer: they enable external access even easier by integrating with an internet facing load balancer which a cloud service may provide!


## Others

- Two types: DNS(preferred) , Env Variables
- you can use `.spec.ipFamilyPolicy` to set if you want dualstack- SingleStack, PreferDualStack, RequireDualStack
-  Use `spec.ipFamilies` to define which IP family you want as default; that one will be selected as default from `.spec.clusterIPs`
-  You may imperetively create svc for a deployment using `kubectl expose deployment <depl_name> --type=<svcType>` , but its highly discouraged, you should use the declaratice approach

