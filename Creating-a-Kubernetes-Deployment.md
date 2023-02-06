# Creating a Kubernetes Deployment 

* We will create a Deployment that will roll out a ReplicaSet to bring up three instances of an nginx Pod. You must have a running Kubernetes cluster with the kubectl command-line tool configured and connected.
* Creating a Deployment is the same as creating a ReplicaSet, but Deployment is the kind property’s value. Moreso, strategy property and values need to be added to the manifest file.
* Create a YAML manifest file named my-deployment.yaml using vi on the command line. Use syntax `vi my-deployment.yaml`
* Copy, paste, and save the configuration below into your YAML file. **The indentation is very Important!**

```
apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: my-deployment 
spec: 
  replicas: 3 
  strategy: 
    type: Recreate 
  selector: 
    matchLabels: 
      app: my-app 
  template: 
    metadata: 
      labels: 
        app: my-app 
    spec: 
      containers: 
      - name: my-deployment-container 
        image: nginx
```
* The above configuration file defined a Deployment object named my-deployment under the metadata.name property with Deployment as the value of its kind property. 
```
spec.replicas: ## the Deployment will create the number of replicated Pods specified in this field
spec.strategy.type: ## the desired strategy type is declared in this field
The strategy is a child of the spec while type is a child of the strategy and grandchild of spec
spec.selector: ## this field defines how the Deployment determines which Pods to manage
spec.containers: ## the container name and image are declared in this field
```
* Run this command to create a Deployment - `kubectl create -f my-deployment.yaml`
* Check the Deployment status to see if it has been created `kubectl get deployments my-deployment`
* When you see an output like this, it means the Deployment is still being created. 
* Wait for a few seconds and run the kubectl get command once again - `kubectl get deployment my-deployment`. 
* The status output is further broken down below:

```
Name: ## Shows the name of the Deployment as declared in the manifest file
Ready: ## This shows the number of replicas that are ready for use. In this case, all 3 out of 3 are ready for use
Up-To-Date: ## This shows the up-to-date number of replicas out of the desired number declared in the YAML file
Available: ## This shows the number of replicas that are available for use 
Age: ## Shows the duration the application has been running
```

* Check the Pod status to confirm if the desired number of Pods are running - `kubectl get pods` 
* To delete one of the instances of the Pod - `kubectl delete pod/podname`
* Check if it has been deleted `kubectl get pods`
* As you would see, three instances of Pod are running while the old ones have already been terminated or deleted. 
* This was made possible with the help of ReplicaSet. 
* One of the importance of ReplicaSet in a Deployment is to ensure that the desire instances of a Pod are up-to-date and are continually running on the cluster. 
* The number of running instances depends on the ReplicaSet value declared in the YAML manifest file.
* To view the description of the Deployment - `kubectl describe deployments my-deployment`
* The Deployment description is further expantiated below:
```
Name: ## The name of the Deployment as specified in the manifest file.
Namespace: ## It is always a default if no namespace is specified in the manifest file.
Replicaset: ## The desired number of replicas specified in the manifest file.
Strategy.recreate: ## The strategy to be used for the Deployment. You can find more info on Deployment strategy in the next part of this series.
Containers.image: ## This field specifies the container and container image names.
Events: ## Shows the Deployment activities since it was created.
```

### Update a Kubernetes Deployment

* You can edit a Deployment by changing the container image from one version to the other, decreasing or increasing the number of instances by changing the ReplicaSet value. etc. For example, the container image nginx we have been using in our exercises has many versions. When no version is specified in the manifest YAML file, the latest version will be pulled from the image repository. We will change the current image version to another as an example.

* **A Deployment can be updated to use a new image in two ways** - 

### Method 1 
You can pass the new image tag directly to the Deployment using flags with the kubectl command-line tool. We will change the nginx in our manifest file to use the nginx:1.18.0 version. 

***
`kubectl --record deployment.apps/my-deployment set image deployment.v1.apps/my-deployment my-deployment-container=nginx:1.18.0`

OR
***
`kubectl set image deployment/my-deployment my-deployment-container=nginx:1.18.0 --record ` 

* In the above command, my-deployment is the name of the Deployment and my-deployment-container is the container name as declared in the YAML file. Check the description again and compare it to the original configuration - `kubectl describe deployment my-deployment`
* Comparing the description outputs of the original and updated versions, we have changed the image version from nginx to nginx:1.18.0 as it is stated in the Pod template. In addition, there are more events compared to before the update describing the internal processes of the rolling update. It shows here that the Deployment updated the Pods by creating and scaling a new ReplicaSet of 3 replicas and destroying the old one. Check the ReplicaSet status for clarity. `kubectl get replicaset`

### Method 2

***

* Directly editing the manifest YAML file. Alternatively, you can edit the Deployment YAML file and change the current image version from nginx:1.18.0 to nginx:1.19.1 by running the below command then save and exit the terminal `kubectl edit deployment.v1.apps/my-deployment`
* Check the status of the Deployment to see that we still have 3 deployments ready `kubectl get deployment my-deployment`
* Check the description of the Deployment to see that the image has been updated `kubectl describe deployment my-deployment`
* Finally, checking the ReplicaSet status, you can see that we created a new Deployment and scaled it up to 3 replicas while the old one was scaled down to 0. Check the ReplicaSet status - `kubectl get replicaset`
* Clean-up: Delete the Deployment using the `kubectl delete` command.









