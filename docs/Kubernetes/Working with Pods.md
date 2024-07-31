#### Introduction:
 
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