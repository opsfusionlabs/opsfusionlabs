
#  Installing Argo CD into EKS cluster


1. Create a name space called **argocd**
```
kubectl create namespace argocd
```   
```
kubectl get ns 
```
2. his command installs the latest stable version of Argo CD.
3. By default Argo Cd come with cluster IP mode, we are going to change form cluster IP to Node port  

