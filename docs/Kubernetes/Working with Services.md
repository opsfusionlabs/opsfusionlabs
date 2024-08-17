
As Pods come-and-go (scaling, failures, rollouts etc.), the Service dynamically updates its list of healthy matching Pods. It does this through a combination of label selection and a construct called an Endpoints object.

Every time you create a Service, Kubernetes automatically creates an associated Endpoints object. The Endpoints object is used to store a dynamic list of healthy Pods matching the Service’s label selector.

> Note: Recent versions of Kubernetes are replacing Endpoints objects with more efficient Endpoint slices. The functionality is identical, but Endpoint slices are higher performance and more efficient.


Accessing Services from inside the cluster


Accessing Services from outside the cluster
