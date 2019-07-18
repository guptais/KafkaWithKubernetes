## Apache kafka on Kubernetes locally

Follow the steps to [Setup] the local cluster. 

Or do these

The Kubernetes package manager

To begin working with Helm, run the ‘helm init’ command:

```
hell init
```

This will install Tiller to your running Kubernetes cluster. It will also set up any necessary local configuration.

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

References:

[How to setup a WebUI Dashboard]

[How to login to kubernetes dashboard secret]


[Setup]: https://medium.com/@tsuyoshiushio/local-kafka-cluster-on-kubernetes-on-your-pc-in-5-minutes-651a2ff4dcde
[How to setup a WebUI Dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[How to login to kubernetes dashboard secret]: http://www.bennythejudge.com/blog/kubernetes/howtos/macos/2018/02/10/kubernetes-dashboard-secret.html
[Kafkacat]: https://docs.confluent.io/current/app-development/kafkacat-usage.html
