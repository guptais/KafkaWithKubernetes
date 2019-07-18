## Apache kafka on Kubernetes locally

Follow the steps to [Setup] the local cluster. 

Or do these

Use Helm, The Kubernetes package manager

To begin working with Helm, run the ‘helm init’ command:

```
hell init
```

This will install Tiller to your running Kubernetes cluster. It will also set up any necessary local configuration.

Then use confluent helm charts by doing the following:

```
git clone https://github.com/confluentinc/cp-helm-charts.git
```

We just override the values which we provide to the helm charts to deploy a kafka cluster within Kubernetes.
Modify the below command based where you have cloned the cp-helm-charts repository.

```
helm install --name my-confluent cp-helm-charts -f ./values.yaml
```

To browse the cluster you can use [Kafkacat] or if you like UIs, you can setup the Kubernetes dashboard

To get it: run the following command

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta1/aio/deploy/recommended.yaml
```

Run the following command in another terminal

```
kubectl proxy
```

Kubectl will make Dashboard available at http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

If this works you will get a login screen.

To login to the k8s cluster, you need the token of the service account of the POD in which the dashboard is running.

Run the following commands:

find your dashboard POD

```
kubectl get pod --namespace=kubernetes-dashboard | grep dash
```

find the serviceAccount for this POD

```
kubectl get pods --namespace=kubernetes-dashboard kubernetes-dashboard-7cf7d57748-c76fh -o yaml | grep -A 15 -B 15 serviceAccount
```

```
kubectl describe secret kubernetes-dashboard --namespace=kubernetes-dashboard
```

From the output of the above command, copy the long string value of token
Field and provide it as the token on the dashboard login page in the browser.

Kubernetes Command cheatsheet:

Cluster, Pod and Container Introspection

```
kubectl get services                # List all services 
kubectl get pods                    # List all pods
kubectl get nodes -w                # Watch nodes continuously
kubectl version                     # Get version information
kubectl cluster-info                # Get cluster information
kubectl config view                 # Get the configuration
kubectl describe node <node>        # Output information about a node
kubectl describe pod <name>              # Describe pod <name>
kubectl get rc                           # List the replication controllers
kubectl get rc --namespace="<namespace>" # List the replication controllers in <namespace>
kubectl describe rc <name>               # Describe replication controller <name>
kubectl get svc                          # List the services
kubectl describe svc <name>              # Describe service <name>

```

Interacting with Pods
```
kubectl run <name> --image=<image-name>                             # Launch a pod called <name> 
                                                                    # using image <image-name>
 
kubectl create -f <manifest.yaml>                                   # Create a service described 
                                                                    # in <manifest.yaml>
 
kubectl scale --replicas=<count> rc <name>                          # Scale replication controller 
                                                                    # <name> to <count> instances
 
kubectl expose rc <name> --port=<external> --target-port=<internal> # Map port <external> to 
                                                                    # port <internal> on replication 
                                                                    # controller <name>
```

Stopping Kubernetes
```
kubectl delete pod <name>                                         # Delete pod <name>
kubectl delete rc <name>                                          # Delete replication controller <name>
kubectl delete svc <name>                                         # Delete service <name>
kubectl drain <n> --delete-local-data --force --ignore-daemonsets # Stop all pods on <n>
kubectl delete node <name>                                        # Remove <node> from the cluster
```

Debugging

```
kubectl exec <service> <command> [-c <$container>] # execute <command> on <service>, optionally 
                                                   # selecting container <$container>
 
kubectl logs -f <name> [-c <$container>]           # Get logs from service <name>, optionally
                                                   # selecting container <$container>
 
watch -n 2 cat /var/log/kublet.log                 # Watch the Kublet logs
kubectl top node                                   # Show metrics for nodes
kubectl top pod                                    # Show metrics for pods
```


References:

[How to setup a WebUI Dashboard]

[How to login to kubernetes dashboard secret]


[Setup]: https://medium.com/@tsuyoshiushio/local-kafka-cluster-on-kubernetes-on-your-pc-in-5-minutes-651a2ff4dcde
[How to setup a WebUI Dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[How to login to kubernetes dashboard secret]: http://www.bennythejudge.com/blog/kubernetes/howtos/macos/2018/02/10/kubernetes-dashboard-secret.html
[Kafkacat]: https://docs.confluent.io/current/app-development/kafkacat-usage.html
