Understanding Kubernetes objects
Kubernetes objects are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster. Specifically, they can describe:
	• What containerized applications are running (and on which nodes)
	• The resources available to those applications
	• The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance
A Kubernetes object is a "record of intent"--once you create the object, the Kubernetes system will constantly work to ensure that object exists. By creating an object, you're effectively telling the Kubernetes system what you want your cluster's workload to look like; this is your cluster's desired state.
To work with Kubernetes objects--whether to create, modify, or delete them--you'll need to use the Kubernetes API. 

Object Spec and Status
Almost every Kubernetes object includes two nested object fields that govern the object's configuration: the object spec and the object status. For objects that have a spec, you have to set this when you create the object, providing a description of the characteristics you want the resource to have: its desired state.
The status describes the current state of the object, supplied and updated by the Kubernetes system and its components. The Kubernetes control plane continually and actively manages every object's actual state to match the desired state you supplied.

Describing a Kubernetes object
When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object (such as a name). When you use the Kubernetes API to create the object (either directly or via kubectl), that API request must include that information as JSON in the request body. Most often, you provide the information to kubectl in a .yaml file. kubectl converts the information to JSON when making the API request.
Here's an example .yaml file that shows the required fields and object spec for a Kubernetes Deployment:

One way to create a Deployment using a .yaml file like the one above is to use the kubectl apply command in the kubectl command-line interface, passing the .yaml file as an argument. Here's an example:
kubectl apply -f https://k8s.io/examples/application/deployment.yaml --record
The output is similar to this:
deployment.apps/nginx-deployment created
Required Fields
In the .yaml file for the Kubernetes object you want to create, you'll need to set values for the following fields:
	• apiVersion - Which version of the Kubernetes API you're using to create this object
	• kind - What kind of object you want to create
	• metadata - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
	• spec - What state you desire for the object
The precise format of the object spec is different for every Kubernetes object, and contains nested fields specific to that object. The Kubernetes API Reference can help you find the spec format for all of the objects you can create using Kubernetes. For example, the spec format for a Pod can be found in PodSpec v1 core, and the spec format for a Deployment can be found in DeploymentSpec v1 apps.

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
