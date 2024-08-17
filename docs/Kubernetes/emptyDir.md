In Kubernetes, `emptyDir` is a type of ephemeral storage that provides a temporary directory shared between all containers in a Pod. This directory is created when the Pod is scheduled to a Node and is deleted when the Pod is removed. The `emptyDir` volume is often used for tasks that require temporary storage, such as caching intermediate files or sharing data between containers in the same Pod.

###  Key Aspects  
1. Lifecycle: The `emptyDir `volume is created as soon as the Pod is assigned to a Node and is destroyed when the Pod is deleted. If a container crashes and restarts within the same Pod, the data in the `emptyDir` persists across container restarts.

2. Storage Medium: By default, `emptyDir` uses the disk of the Node where the Pod is running. You can also specify that the `emptyDir `should be memory-backed by setting the medium field to `"Memory"`, which causes the data to be stored in RAM. This can provide faster access but at the cost of using up Node memory.
   
3. **Usage Scenarios**:

	1. **Scratch Space**: Temporary files that do not need to persist beyond the Pod's lifetime.
	2. **Data Sharing**: Sharing files between containers in the same Pod.
	3. **Caching**: Storing intermediate data that can be safely discarded once the Pod is terminated.

###  Declaratively

- #### Empty dir of single pod 

	```yaml title="ofl-empty-dir-volume-mount.yml" linenums="1" hl_lines="10 13"
	apiVersion: v1
	kind: Pod
	metadata:
	    name: ofl-empty-dir-pod
	spec:
	    containers:
	    - name: ofl-empty-dir-pod
	      image: busybox
	      command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 10; done']
	      volumeMounts:
	      - name: output-vol
	        mountPath: /output
	    volumes:
	    - name: output-vol
	      hostPath:
	        path: /var/datas
	```


- Run the following command to create `ofl-empty-dir-pod`

	```
	kubectl apply -f ofl-empty-dir-volume-mount.yml
	```

	- #####  Inspect empty dir

		- Run the following command to list the pod
		
		```
		kubectl get pod 
		```
		
		- Run the following command to describe the pod ``ofl-empty-dir-pod``
		
		```
		kubectl describe pod ofl-empty-dir-pod
		```
		
		- Run the following command to  deep dig inside the pod analyses the volume behavior
		
		```
		kubectl exec -i -t ofl-empty-dir-pod -- /bin/sh
		```
		


- #### Empty dir  multi container pod

	```yaml title="ofl-empty-dir-shared-volume-multi-mount.yml" linenums="1" hl_lines="11 12 13 19 20 21 27 28 29"
	
	apiVersion: v1
	kind: Pod
	metadata:
	  name: ofl-empty-dir-shared-volume
	spec:
	  containers:
	  - name: myvolumes-container-1
	    image: alpine
	    command: ['sh', '-c', 'while true; do echo Success! >> output1.txt; sleep 3600; done']
	    
	    volumeMounts:
	    - mountPath: /demo1
	      name: demo-volume
	
	  - name: myvolumes-container-2
	    image: alpine
	    command: ['sh', '-c', 'while true; do echo Success! >> output2.txt; sleep 3600; done']
	    
	    volumeMounts:
	    - mountPath: /demo2
	      name: demo-volume
	
	  - name: myvolumes-container-3
	    image: alpine 
	    command: ['sh', '-c', 'while true; do echo Success! >> output3.txt; sleep 3600; done']
	    
	    volumeMounts:
	    - mountPath: /demo3
	      name: demo-volume
	
	  volumes:
	  - name: demo-volume
	    emptyDir: {}
	
	
	```


- Run the following command to create `ofl-empty-dir-shared-volume`

	```
	kubectl apply -f ofl-empty-dir-shared-volume-multi-mount.yml
	```

	- #####  Inspect empty dir

		- Run the following command to list the pod
		
		```
		kubectl get pod 
		```
		
		- Run the following command to describe the pod ``ofl-empty-dir-pod``
		
		```
		kubectl describe pod ofl-empty-dir-shared-volume
		```
		
		- Run the following command to  deep dig inside the pod analyses the volume behavior
		
		```
		kubectl exec -i -t ofl-empty-dir-shared-volume -c myvolumes-container-1 -- /bin/sh
		```

		```
		kubectl exec -i -t ofl-empty-dir-shared-volume -c myvolumes-container-2 -- /bin/sh
		```

		```
		kubectl exec -i -t ofl-empty-dir-shared-volume -c myvolumes-container-3 -- /bin/sh
		```

> 	Note: All containers in a Pod have read/write access to the same emptyDir - if they requested a mount point for it. Containers can access the emptyDir using the same or different mount points.


## Clean up 

- Run the following command to clear all deployment in cluster
  
	```
	kubectl delete all --all
	```


