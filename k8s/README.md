Kubernetes Learning

A POD is an object which wraps over a collection of  containers that run on a sigle host. Most often you would find just 1 container running on a POD.

POD is the smallent object which you can deploy on k8s. Also PODs are ephemeral in name, i.e. they can fail and will fail. When a pod fails, k8s will not repair it, instead just start a new one! Your services should be designed keeping this reality in mind.

Pods enable scheduling (using affinity), resource sharing between its containers. 

Every pod creates its own net namespace i.e. gets its own IP which is fully routable on the internal k8s pod network. Each container in the Pod shares the IP address assigned to the POD.

You should always scale the number of PODs not the number of containers in the PODs.

To know more about PODs try the `kubectl explain pods` command. Here listing down a few key options:'

pods.Metadata:
- Name: A unique name for the pod, this is required, however some resources(like deployments) may assign this name automatically; your containers will inherit this as their hostname.
- Labels:  They are the map of the strings which k8s uses to select/organise and categorise pod objects.
- Annotations: They are KVP which can be used to add new external tools like spinnaker 

pods.spec
- Affinity:  defines where should the pods run on a cluster; i..e same pod, same AZ etc
- Probles: helps run health check and control pod restart on unhealthy state
-

Pod Lifecycle:
- Simple repn: Pending > Running > Succeeded (for short living pod) 
- Long running: define restart policy on failure; controlled via Deployments, Statefulsets and Daemon sets
- Short running: runs and terminates, no restart; controlled via Jobs and CronJobs

Multi container Pod patterns:
- Sidecar:  most generic pattern, one container is helper responsible for syncing data
- Adapter:  like sidecar but sequential; i.e. helper container gets data from main container which it would process
- Ambassador:  like sidecar, but helper server which can help communicate with external systems
- Init: Runs to initialise environment for the main container.


Cool Commands:

- `kubectl explain pods.{resourcePath}`  : your man page
- `kubectl apply -f {fileName}.yaml` : POSTs the POD YAML file to API server; dont do this use Deployments instead
- `kubectl get pods ` : Get  pod running info, but very limited data
    - `-w`  : follows the pod deployment flow, command wont end and display status changes
    - `-o wide` : displays,  IP, Node, etc along with basic stuff
    - `-o yaml` : full detailed dump of the pod object  with the desired(.spec) and observed(.status) states.
- `kubectl describe pods <pod_name>` : gives a nice human readable output on the status of the pod; includes pod env vars; and events which have happened while booting the pod
- `kubectl logs <pod_name>` :  returns the Stdout and stderr data; use `-f` to follow the logs; use `--tail N` to get the last N log lines
- `kubectl exec <pod_name>  -- <cmd>` : executes the `cmd` on the pod; use `-it` flag and `/bin/bash` or `sh` as cmd with the exec command to get access to the pod shell.
- `kubectl delete <pod_name>` : deletes a given pod ; note: your deployment or other objects may recreate the pods
