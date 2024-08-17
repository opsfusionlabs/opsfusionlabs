Namespaces are a native way to divide a single Kubernetes cluster into multiple virtual clusters.

- Run the following command to list all Kubernetes API resources

	```
	kubectl api-resources
	```

- Namespaces a good way of sharing a single cluster among different departments and environments. For example, 

- A single cluster might have the following Namespaces.
	- Dev
	- Test
	- QA
## Creating Namespaces

- ### Imperatively
  
- Run the following command to create namespace called `opsfulsionlabs`
  
	```
	kubectl create ns opsfulsionlabs
	```

Declaratively

- Run the following command to create namespace called `opsfulsionlabs`

	``` yaml title="Namespace/ofl-namespace.yml" linenums="1" hl_lines="4"
	apiVersion: v1
	kind: Namespace
	metadata:
	  name: opsfusionlabs
	  labels:
	    name: OpsFusionLabs
	    env: dev
	
	```

	```
	kubectl apply -f ofl-namespace.yml
	```


- Auto Generate Namespace
  
	- ######  Ns mainfest file in **YML**
		```
		kubectl create namespace opsfusionlabs-yml -o yaml | tee ofl-namespace-autogenerate.yml
		```
	
	
	- ###### Ns mainfest file in **JSON**
	
		```
		kubectl create namespace opsfusionlabs-json -o json | tee ofl-namespace-autogenerate.json
		```


### Inspecting Namespaces

- Run the following command to list all namespaces

	```
	kubectl get namespaces 
	```

	```
	kubectl get ns 
	```

- Run the following command to describe namespaces
	
	```
	kubectl describe ns deafult 
	```

### Deleting Namespace

- Imperatively

	- Run the following command to delete namespaces 

		```
		kubectl delete ns <name od the namespace> 
		```

- Declaratively
  
	- Run the following command to delete namespaces 

		```
		kubectl delete -f ofl-namespace.yml
		```


### Set New Default Namespace

When you start using Namespaces, you’ll quickly realize it’s painful remembering to add the `-n` or `--namespace` flag on all `kubectl` commands. A better way might be to set your `kubeconfig` to automatically work with a particular Namespace.

- Run the following command to change namespaces **default** to **opsfusionlabs**

	```
	kubectl config set-context --current --namespace opsfusionlabs
	```

- Run the following command to list the objects in opsfusionlabs namespace

	```
	kubectl get all 
	```


### Mapping Objects in Specific Namespace

- #### Imperatively
  
	- Run the following command to create nginx pod in  `opsfusionlabs` namespace 

		```
		kubectl run ofl-imp-nginx --image=ingnix:latest -n opsfusionlabs
		```

- #### Declaratively

	- Run the following command to create nginx pod in  `opsfusionlabs` namespace 

		```
		kubectl apply -f ofl-mapping-ns.yml
		```

		``` yaml title="Namespace/ofl-mapping-ns.yml" linenums="1" hl_lines="9"
		
		apiVersion: v1
		kind: Pod
		metadata: 
		  name: ofl-ns-mapping
		  labels:
		    name: PodwithPort
		    environment: production
		    tier: front-end
		  namespace: opsfusionlabs
		spec:
		  containers:
		  - name: nginx
		    image: nginx:latest
		    ports:
		    - containerPort: 80
		```


## Clean up

- Run the following command to clear all deployment in cluster
  
	```
	kubectl delete all --all -n opsfusionlabs
	```
