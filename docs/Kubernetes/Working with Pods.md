### Introduction
 
  Pods are the Smallest Deployable Units of computing that can be created and managed in Kubernetes. Pods with super-powers such as self-healing, scaling, updates and rollbacks. 
  
  Some quick examples If we want to deploy an app, you deploy it in a Pod. If you terminate an app, you terminate its Pod. If you scale an app up or down, you add or remove Pods.

Why Pods ? 

1. Write your app/code 
2. Package it as a container image 
3. Wrap the container image in a Pod
4. Run it on Kubernetes
   
> Note:  Kubernetes doesn’t allow containers to run directly on a cluster, they always
> have to be wrapped in a Pod.

1. Pods augment containers
2. Pods assist in scheduling
3. Pods enable resource sharing

Pods augment containers 

• Labels and annotations
• Restart policies
• Probes (startup probes, readiness probes, liveness probes, and potentially more)
• Affinity and anti-affinity rules
• Termination control
• Security policies
• Resource requests and limits


Deploy Pod using Imperative method
Deploy a Pod using Declarative method
Generate the pod manifest in the YAML and JSON format
Reading the Pod's information and metadata
Listing the objects output in JSON or YAML format
Entering into a container in a Pod
Static Pod
Multi-Container Pod
Sidecar Container
Deleting a Pod


### Handson 

Imperative Method

Create a pod with Imperative Method

```
kubectl run <desired-pod-name> --image <Container-Image> --generator=run-pod
```

```
  kubectl run web01 --image nginx
```

list the pod status

```
 kubectl get pods
```



List Pods with wide option

List pods with wide option which also provide Node information on which Pod is running

```
kubectl get pods -o wide
```






Declarative Method

Run a kubectl explain pods command to list all possible Pod attributes. Beware, the command returns over 1,000 lines and the following output has been trimmed.

```
kubectl explain pods --recursive
```


