**Deployments** in Kubernetes is a crucial part of managing applications in a cluster. Deployments provide a higher-level abstraction over ReplicaSets, offering additional features like rolling updates, rollbacks, and easier scaling. Here’s a guide to understanding and working with Kubernetes Deployments 

## Why Deployment ? 

A **Deployment** is a Kubernetes resource that manages a ReplicaSet and provides declarative updates to applications. It ensures that a specified number of pod replicas are running at any given time and can automatically roll out changes to your application in a controlled way.

### Key Features 

- **Rolling Updates**: Gradually update pods with new versions while maintaining application availability.
- **Rollbacks**: Revert to a previous stable version if something goes wrong during an update.
- **Declarative Updates**: Simply update the Deployment configuration, and Kubernetes handles the rest.
- **Scaling**: Easily scale the number of replicas up or down.
- **Self-Healing**: Automatically replaces failed pods to maintain the desired state.

## Imperative Method

- Create Deployment using `nginx:latest`  image

	```
	kubectl create deployment nginx-deployment --image=nginx:latest
	```

### Inspecting Deployments

- Describe deployment

```
kubectl describe deployment nginx-deployment 
```

```
kubectl get all
```

- Delete Deployment 

```
kubectl delete deployment nginx-deployment 
```

### Auto Generate Mainfest Files 

- Create deployment of `httpd` web server with 5 **replicas** in yaml 

	```
	kubectl create deployment httpd-deployment --image=httpd --replicas=5 --dry-run=client -o yaml | tee httpd-deployment.yml
	```

- Deploy the `httpd-deployment.yml` main file 

	```
	kubectl apply -f httpd-deployment.yml
	```

	```
	kubectl get all
	```

## Declarative Method

- Example Configuration of `nginx` deployment file 

	```yaml title="Deployments/ofl-deployment.yml" linenums="1" hl_lines="2"
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: ofl-nginx-deployment
	spec:
	  replicas: 3
	  selector:
	    matchLabels:
	      app: ofl-nginx-webserver
	  template:
	    metadata: 
	      name: ofldeploymnet
	      labels:
	        app: ofl-nginx-webserver
	        env: production
	        tier: front-end
	    spec:
	      containers:
	      - name: ofl-nginx-container
	        image: nginx:latest
	        ports:
	          - containerPort: 80
	
	```


	**Explanation**:

	- **replicas**: Specifies that three pod replicas should be running.
	- **selector**: Matches pods with the label `app: ofl-nginx-webserver.
	- **template**: Defines the pod specification, including container images and other configurations.

	- Create `ofl-deployment
	
		```
		kubectl apply -f ofl-deployment.yml
		```
	
	- List of the Deployments
	
		```
		kubectl get deployment -o wide
		```
	
	- Describe Deployments
	
		```
		kubectl describe deployment ofl-nginx-deployment
		```
	
	- List out the all objects 
	
		```
		kubectl get all 
		```

---
## Perform scaling operations


Manually scaling the number of replicas in a Deployment is easy. You can do it imperatively with the `kubectl` scale command, or declaratively by updating the YAML file and re-posting to the API server. You’ll do it both ways, but the preferred way is the declarative way.


- First Verify the current number of replicas.

	```
	kubectl get deploy ofl-nginx-deployment
	```

- #### Scale Up

	- Run the following command to scale up to 5 pods and verify the operation.
	
		```
		kubectl scale deploy ofl-nginx-deployment --replicas 5
		```
	
		```
		kubectl get deploy ofl-nginx-deployment
		```

- #### Scale Down

	- Run the following command to scale Down to 3 pods and verify the operation.

		```
		kubectl scale deploy ofl-nginx-deployment --replicas 3
		```
	
		```
		kubectl get deploy ofl-nginx-deployment
		```

	- Run the following command to list the replicates 

		```
		kubectl get rs 
		```

- #### Self Healing 

	- Run the following command to list the pods

		```
		kubectl get pods 
		```

	- Run the following command to delete pods 

		```
		kubectl delete pod --all
		```

	- Run the following command to list the pods

		```
		kubectl get pods 
		```

## Perform a rolling update

 - To perform a rolling update on the app already deployed. We’ll assume the new version of the app has already been created and containerized as an image with the 2.0 tag. 
 
 - When we “update” a Pod, we’re actually terminating it and replacing it with a new one. Pods are designed as **immutable objects**, so we never change or update existing ones.

- Update the image version in the Deployment’s resource manifest. The initial release of the app is using the `httpd:2.4.6` image. Update that to reference the newer `httpd:latest` mage and save your changes. This ensures next time the manifest is posted to the API server, all Pods managed by the Deployment will be replaced with new ones running the new 2.0 image.


> **Terminology:** We often use the terms update, rollout, and release to mean the same thing – issuing a new version of an app.

- Example Configuration of `httpd:1.0` deployment file 

	```yaml title="Deployments/ofl-rolling-update.yml" linenums="1" hl_lines="10 11 12 13 14 15 16 17"
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: ofl-httpd-deployment
	spec:
	  replicas: 3
	  selector:
	    matchLabels:
	      app: ofl-httpd-deployment
	  revisionHistoryLimit: 5
	  progressDeadlineSeconds: 300
	  minReadySeconds: 10
	  strategy:
	    type: RollingUpdate
	    rollingUpdate:
	      maxUnavailable: 1
	      maxSurge: 1
	  template:
	    metadata: 
	      name: ofldeploymnet
	      labels:
	        app: ofl-httpd-deployment
	        env: production
	        tier: front-end
	    spec:
	      containers:
	      - name: ofl-httpd-container
	        image: httpd:1.0
	        ports:
	          - containerPort: 80
	        resources:
	          requests:
	            memory: "64Mi"
	            cpu: "250m"
	          limits:
	            memory: "128Mi"
	            cpu: "500m"
	
	```

- `revisionHistoryLimit` tells Kubernetes to keep the configs of the previous 5 releases. This means the previous 5 ReplicaSet objects will be kept and you can rollback to any of them.

- `progressDeadlineSeconds` governs how long Kubernetes waits for each new Pod replica to start, before considering the rollout to have stalled. This config gives each Pod replica its own 5 minute window to come up.

- .`spec.minReadySeconds` throttles the rate at which replicas are replaced. The one in the example tells Kubernetes that any new replica must be up and running for 10 seconds, without any issues, before it’s allowed to update/replace the next one in sequence. Longer waits give you a chance to spot problems and avoid updating all replicas to a dodgy version. In the real world, you should make the value large enough to trap common failures.
	- `.spec.strategy`
	  
		- Update using the `RollingUpdate` strategy
		- Never have more than one Pod below desired state (maxUnavailable: 1)
		- Never have more than one Pod above desired state (maxSurge: 1)

	- Run the following command to create `ofl-rolling-update` deployment 
	
		```
		kubectl apply -f ofl-rolling-update.yml
		```
	
	- Check rollout history of the `ofl-httpd-deployment`
	
		```
		kubectl rollout history deployment ofl-httpd-deployment
		```
	
		``` title="Output of kubectl rollout history deployment ofl-httpd-deployment"
		deployment.apps/ofl-httpd-deployment 
		REVISION  CHANGE-CAUSE
		1         <none>
		
		```

	- Check the rollout status 
		```
		kubectl rollout status deployment ofl-httpd-deployment
		```
		
	- Pausing and resuming rollouts
	
		```
		kubectl rollout pause deploy ofl-httpd-deployment
		```
	
		```
		kubectl rollout resume deploy ofl-httpd-deployment
		```
	
	- Once it completes, you can verify with` kubectl `get deploy
	
		```
		kubectl get deploy ofl-httpd-deployment
		```

### Rollout a Deployment

- Now deploy the new image with tag `2.0`

	```yaml title="Deployments/ofl-rolling-update.yml" linenums="1" hl_lines="4"
	    spec:
	      containers:
	      - name: ofl-httpd-container
	        image: httpd:2.0 # Updating from latest to version 1
	        ports:
	          - containerPort: 80
	        resources:
	```

- Run the following command to update `ofl-rolling-update` deployment  
  
	```
	kubectl apply -f ofl-rolling-update.yml
	```
	
- Run the following command to check rollout history

	```
	kubectl rollout history deployment ofl-httpd-deployment
	```

> 	Revision 1 was the initial deployment that used the 1.0 image tag. Revision 2 is the rolling update you just performed.
### Rolling Back a Deployment

- If something goes wrong after an update, you can roll back the Deployment to a previous revision.

	```
	kubectl rollout undo deployment ofl-httpd-deployment  --to-revision=1
	```
<center> or </center> 

	```
	kubectl rollout undo deployment/ofl-httpd-deployment`
	```
	> This command rolls back the Deployment to the previous stable version.
	

## Clean up 

- Run the following command to clear all deployment in cluster
  
	```
	kubectl delete all --all
	```
