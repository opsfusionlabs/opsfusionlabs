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

- Labels and annotations
- Restart policies
- Probes (startup probes, readiness probes, liveness probes, and potentially more)
- Affinity and anti-affinity rules
- Termination control
- Security policies
- Resource requests and limits


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

### Imperative Method

1. Create a pod with Imperative Method

	```
	kubectl run <desired-pod-name> --image <Container-Image> --generator=run-pod
	```
	
	```
	kubectl run webserver --image nginx
	```

#### Introspecting running Pods

2. List the pod status

	```
	 kubectl get pods
	```

3. List Pods with **wide** option **-o wide** gives a couple more columns but is still a single line of output
   
	```
	kubectl get pods -o wide
	```

4. Get complete pod in details 
   
	```
	kubectl describe pod <pod name>
	```

	```
	kubectl describe pod webserver
	```


5. Deleting a Pod 

	```
	kubectl delete <pod name>
	```

	```
	kubectl delete pod webserver
	```

### Declarative Method

For the declarative method, we use YAML (Yet Another Markup Language) files to present the configuration of what we desire to be the end product.

Run a **kubectl** explain pods command to list all possible Pod attributes. <br>
Beware, the command returns over 1,000 lines and the following output has been trimmed.

```
kubectl explain pods --recursive
```

```
kubectl explain pod.spec.restartPolicy
```

#### Pod manifest files 

 In order to create a pod using a declarative method, we need to create a yaml file and prepare a template.
 
Example Configuration

```yaml 
---
apiVersion: v1
kind: Pod
metadata:
  name: firstpod
spec:
  containers:
    - name: nginx
      image: nginx:latest
```


- The **.apiVersion** defines the schema version to use when creating the object. This file is defining a Pod object and telling Kubernetes to build it using the v1 Pod schema.
	- The normal format for apiVersion is **api-group/version**
	
	- However, Pods are defined in a special API group called the core group which omits the api-group part. 
	
	- For example, StorageClass objects are defined in the v1 schema of the storage.k8s.io API group and are described in YAML files as **storage.k8s.io/v1.** 
	  
- The **.kind** field tells Kubernetes the type of object being defined. This file is defining a Pod object.
- The **.metadata** section is where you attach things such as names, labels, annotations, and a Namespace.
- The **.spec** section is where you define the containers the Pod will run. This is called the Pod template, and this example is defining a single-container Pod based on the **nginx:latest** image.

#### Dry Run

A dry run is a way to simulate the execution of a command without actually applying any changes.

You can use the `--dry-run` flag with various `kubectl` commands to perform a dry run. As of Kubernetes 1.18 and later, the `--dry-run` flag has been updated to take two options: `client` and `server`.

- **`--dry-run=client`**: This performs the dry run on the client side without sending the request to the API server. This was the default behavior in Kubernetes 1.17 and earlier.
    
- **`--dry-run=server`**: This performs the dry run on the server side, meaning it sends the request to the API server, which then validates the request and simulates the changes without persisting them. This is the preferred option in newer versions, as it provides more accurate validation.

```
kubectl run webserver --image nginx --dry-run=client
```

Dry Run Benefits

- Validation: Ensure your **manifests are correct** and will be accepted by the API server.
- Safety: Avoid **unintended** changes to the live environment.
- Automation: Use dry runs as part of **CI/CD pipelines to validate configurations** before deploying them.
#### Auto Generate Pod manifest files

we can generate the pod manifest in two different formats 

1. YML
2. Json
##### Pod manifest in **yml** 

```
kubectl run webserver1 --image nginx --dry-run=client -o yaml | tee webserver1.yml
```

- Create a pod containing a **nginx** server using webserver1.yml file 

	```
	kubectl apply -f webserver1.yml
	```

##### Pod manifest in **Json** 

```
kubectl run webserver2  --image httpd --dry-run=client -o json | tee webserver2.json
```

- Create a pod containing a **httpd** server using webserver2.json file 

	```
	kubectl apply -f webserver2.json
	```


### Labels

labels are key-value pairs that are attached to objects, such as Pods, Services, and Nodes. Labels are used to organize and select subsets of objects. They are a powerful way to manage and query resources in your Kubernetes cluster.

- **Key:** A string that identifies the label. Keys can be up to 63 characters long, must begin and end with an alphanumeric character, and can include dashes (`-`), underscores (`_`), and dots (`.`). Optionally, keys can be prefixed with a domain name followed by a slash, allowing for more organizational control (e.g., `app.kubernetes.io/name`).
    
- **Value:** A string that is associated with the key. Values can be up to 63 characters long and must adhere to the same character rules as keys.
    
- **Selectors:** Labels are often used in conjunction with selectors to query or filter resources. For example, you can create a Service that selects all Pods with a specific label.

Example Configuration 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: podwithlabel
  labels:
    name: PodwithLabel
    environment: production
    tier: front-end
spec:
  containers:
  - name: nginx
    image: nginx:latest
```


- Create Pod without label: 
  
	```
	kubectl apply -f 01-pod.yml
	```

- Create pod with labels 

	```
	kubectl apply -f 02-PodwithLabel.yml
	```

- list pods with labels 

	```
	kubectl get pods --show-labels 
	```

- Add or override the labels to the existing pods 

	```
	kubectl label pod webserver2 environment: production
	```

- Listing Pods with a Specific Label

	```
	kubectl get pods -l environment=production
	```


- Delete pod with label 

	```
	kubectl delete pod -l environment=production
	```

> *Note: Spin up `01-pod.yml` and `02-PodwithLabel.yml`*

Common Uses of Labels to Organizing resources, Targeting resources, Filtering and Monitoring and Logging

### Resource requests

- **Resource Requests** define the minimum amount of **CPU** or **Memory** that a container is guaranteed to have.
- Kubernetes uses the request value to determine how to schedule the container onto a node. The scheduler ensures that the node has at least the requested amount of resources available.

	```
	kubectl apply -f 04-PodwithResoucesLimit.yml
	```

	```
	kubectl describe pod podwithresourceslimit
	```
### Resource Limits

- **Resource Limits** define the maximum amount of CPU or memory that a container is allowed to use.
- If a container tries to use more than the limit, Kubernetes may throttle the CPU or, in the case of memory, terminate the container.

	```
	kubectl apply -f 05-PodwithResourceRequestandLimit.yml
	```

	```
	kubectl describe pod podwithresourcesrequestandlimit
	```

Example Configuration 

```yaml 
apiVersion: v1
kind: Pod
metadata:
  name: podwithresourcesrequest
  labels:
    name: podwithresourcesrequest
    environment: production
    tier: front-end
spec:
  containers:
  - name: podwithresourcesrequest
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

- **Requests:**
    - `memory: "64Mi"` means the container is guaranteed 64 MiB of memory.
    - `cpu: "250m"` means the container is guaranteed 250 millicores (0.25 of a CPU core).
- **Limits:**
    - `memory: "128Mi"` means the container can use up to 128 MiB of memory.
    - `cpu: "500m"` means the container can use up to 500 millicores (0.5 of a CPU core).

####  Best Practices

- **Set Requests Based on Baseline Usage:** Set resource requests based on the average resource usage of your application. This ensures that the application always gets the resources it needs to function properly.
    
- **Set Limits Based on Peak Usage:** Set resource limits slightly above the expected peak usage to allow for short bursts of resource consumption without risking throttling or OOM kills.
    
- **Monitor and Adjust:** Regularly monitor the resource usage of your applications and adjust the requests and limits as necessary to optimize performance and resource utilization.
    

Using resource requests and limits effectively ensures that your applications run smoothly, that resources are used efficiently, and that the cluster remains stable and performant.


### Multi-Container Pod

 **Multi-container Pod** in Kubernetes is a Pod that runs more than one container. These containers share the same network namespace, storage volumes, and can communicate with each other directly using `localhost`. Multi-container Pods are typically used when you have tightly coupled processes that need to work together within the same Pod.

Why Use Multi-Container Pods?

Multi-container Pods are useful in scenarios where:

1. **Sidecar Containers:** A container that provides supporting functionality for the main application, such as logging, monitoring, or data synchronization.
    
2. **Ambassador Containers:** A container that acts as a proxy or helper for external communication, simplifying interactions with external services.
    
3. **Adapter Containers:** A container that modifies or adapts the output of another container, such as transforming logs or data.

Example Configuration 

```yaml 
apiVersion: v1
kind: Pod
metadata:
  name: multicontainerpod
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
  - name: sidecar-logger
    image: busybox
    command: ["sh", "-c", "while true; do echo $(date) - Log from sidecar; sleep 100; done"]
```


- Create Multi-Container pod 

	```
	kubectl apply -f 06-PodwithMultiContainer.yml
	```

- Describe the multi container pod 

	```
	kubectl describe pod multicontainerpod
	```

- Check the container logs 

	```
	kubectl logs pod/multicontainerpod
	```

#### Use Cases 

- **Microservices**: When you have two tightly coupled microservices that need to share resources or communicate frequently.
- **Service Mesh**: Sidecar containers are often used in service mesh architectures to handle networking tasks such as load balancing, traffic management, and security.
- **Data Processing Pipelines**: Where one container handles data ingestion, another processes the data, and yet another exports the processed data.
### Connect to Container in a POD

To connect to a container within a Kubernetes Pod, you typically use the `kubectl exec` command. This command allows you to run commands inside a container, effectively giving you access to the container's shell or to execute a specific command inside it.

```
kubectl exec -it <pod-name> -- <command>
```


- **`-i`**: Stands for "interactive." It keeps the session open, allowing you to interact with the container.
- **`-t`**: Stands for "TTY." It allocates a terminal for the session, which is useful for running interactive commands like a shell.
- **`<pod-name>`**: The name of the Pod you want to connect to.
- **`<command>`**: The command you want to run inside the container, such as `sh` or `bash` for a shell.

```
kubectl exec -it podwithlabel -- /bin/bash
```

- Execute some commands in Nginx container
  
	```
	ls
	cd /usr/share/nginx/html
	cat index.html
	```

- Running individual commands in a Container
  
	```
	kubectl exec -it podwithlabel env
	kubectl exec -it podwithlabel ls
	kubectl exec -it podwithlabel cat /usr/share/nginx/html/index.html
	```

- Specifying a Container in a Multi-Container Pod

	If your Pod has multiple containers, you need to specify which container you want to connect to using the `-c` flag:
	
	```
	kubectl exec -it <podname> -c <container-name> -- /bin/bash
	```

	```
	kubectl exec -it multicontainerpod -c nginx-container -- /bin/bash
	```


### Pod With Port Number

In Kubernetes, we can specify the port numbers to containers use within a Pod by defining them in the Pod's YAML manifest. This allows you to expose specific ports for your application to handle traffic.

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: podwithport
  labels:
    name: PodwithPort
    environment: production
    tier: front-end
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

**Ports:** A list of ports that the container will expose. **ContainerPort:** The port that the container will expose (`80` for HTTP traffic in this example).

- Create pod with container Single port number 

	```
	kubectl apply -f 03-PodwithPort.yml
	```

- Describe the pod 

	```
	kubectl describe pod podwithport
	```

- Create pod with container Multiple port number 

	```
	kubectl apply -f 03.1-PodwithMultiPort.yml
	```

- Describe the pod 

	```
	kubectl describe pod podwithmultiport
	```

- Get the Pod IP 

	```
	kubectl get pod podwithport -o jsonpath='{.status.podIP}'
	```

- Assign POD IP to Variable 
  
	```
	PodIP=$(kubectl get pod podwithport -o jsonpath='{.status.podIP}')
	```

	```
	echo $PodIP
	```

##### Clean up 

- [x] Remove the all pods at a time 

	```
	kubectl delete pods --all 
	```