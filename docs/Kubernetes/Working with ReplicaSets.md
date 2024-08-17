**ReplicaSets** in Kubernetes are a type of controller that ensures a specified number of identical Pod replicas are running at any given time. They help manage the availability of your applications by automatically creating or deleting Pods as necessary to match the desired number of replicas.

### Key Concepts

- **Desired State:** The number of Pod replicas that you want running in your cluster.
- **Current State:** The number of Pod replicas currently running in your cluster.
- **Self-Healing:** If a Pod fails or is deleted, the ReplicaSet controller automatically creates a new Pod to maintain the desired state.

#### Core Components of RS :

- **Specifying Replicas**:
    
    - The `replicas` field in the ReplicaSet manifest defines how many replicas of a pod should be running. For example, setting `replicas: 4` ensures that four instances of the pod are running at all times.
- **Pod Template**:
    
    - The `template` section of the ReplicaSet defines the pod specification. This includes container images, environment variables, ports, and other configurations.
- **Label Selector**:
    
    - The `selector` section is used to match the labels on pods. It determines which pods the ReplicaSet is responsible for. This allows the ReplicaSet to manage specific pods based on their labels.
- **Self-Healing**:
    
    - ReplicaSets are self-healing by design. If a pod dies or becomes unresponsive, the ReplicaSet replaces it automatically, ensuring the system remains in the desired state.

Example Configuration

```yaml title="Replicasets/ofl-replicaset.yml" linenums="1" hl_lines="6 7 8 9 13"
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: ofl-replicaset
spec:
  replicas: 4
  selector:
    matchLabels:
      environment: production
  template:
    metadata:
      labels: 
        environment: production
        tier: front-end
    spec:
      containers:
        - name: ofl-webserver
          image: nginx:latest
          ports:
          - containerPort: 80

```

  

- Create ReplicaSet 

	```
	kubectl apply -f Replicasets/ofl-replicaset.yml
	```

- List of the ReplicaSet 

	```
	kubectl get rs
	```

- Describe ReplicaSet 

	```
	kubectl describe replicaset ofl-replicaset
	```

- List out the all objects 

	```
	kubectl get all
	```

### Scaling ReplicaSets

- **Scaling ReplicaSets** in Kubernetes involves adjusting the number of pod replicas that are running at any given time. This can be done manually or automatically, depending on your needs.

#### Manual Scaling 

You can manually scale a ReplicaSet by modifying the `replicas` field in the ReplicaSet YAML manifest or by using the `kubectl` command.

- Scaling via `kubectl` Command

- The easiest way to scale a ReplicaSet is to use the `kubectl scale` command. This command allows you to specify the desired number of replicas.

	```
	kubectl scale rs <rs-name> --replicas=5
	```

	```
	kubectl scale rs ofl-replicaset --replicas=5
	```

- Scaling by Editing the YAML Manifest. 
	- You can also scale a ReplicaSet by editing its YAML manifest directly and applying the changes.
	- Update the `replicas` field in the YAML file using `kubectl` command

		```
		kubectl edit rs ofl-replicaset
		```

	- Update the `replicas` field in the YAML file.

		```yaml title="Replicasets/ofl-replicaset.yml" linenums="1" hl_lines="6"
		apiVersion: apps/v1
		kind: ReplicaSet
		metadata:
		  name: ofl-replicaset
		spec:
		  replicas: 4 # Change this number to the desired replica count
		  selector:
		    matchLabels:
		      environment: production
		  template:
		    metadata:
		      labels: 
		        environment: production
		        tier: front-end
		    spec:
		      containers:
		        - name: ofl-webserver
		          image: nginx:latest
		          ports:
		          - containerPort: 80
		```

		- Apply the updated YAML file:

		```
		kubectl apply -f Replicasets/ofl-replicaset.yml
		```

		- Update replica set using `sed` command 

		```
		sed -i 's/4/6/g' Replicasets/ofl-replicaset.yml
		```

#### Automatic Scaling 

For dynamic scaling based on resource utilization (e.g., CPU, memory), you can use the **Horizontal Pod Autoscaler (HPA)**. The HPA automatically scales the number of pods in a ReplicaSet (or Deployment) based on observed CPU utilization, memory usage, or custom metrics.

```
kubectl autoscale rs ofl-replicaset --min=2 --max=10 --cpu-percent=80
```

- `--min=2`: Minimum number of replicas.
- `--max=10`: Maximum number of replicas.
- `--cpu-percent=80`: Target average CPU utilization across all pods.

You can check the status of the HPA using:

```
kubectl get hpa
```

The HPA will monitor the resource utilization of the pods managed by the ReplicaSet and adjust the number of replicas automatically within the specified range.


#### CleanUp

- Delete `ofl-replicaset`

	```
	kubectl delete -f Replicasets/ofl-replicaset.yml
	```
	
	```
	kubectl delete hpa ofl-replicaset
	```

