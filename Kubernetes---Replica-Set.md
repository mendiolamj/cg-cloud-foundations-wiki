# Create a Kubernetes Replica Set 

* Create a YAML file using vi on the command line - `vi replicaset-app.yaml`
* Copy, paste, and save the configuration below into your YAML file. **The indentation is very Important!**

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
```
* Create the ReplicaSet with this command - `kubectl create -f replicaset-app.yaml`
* Once the ReplicatSet is running, you can check its status - `kubectl get replicaset my-replicaset`
* Next, you can check the status of the Pods running in the ReplicaSet - `kubectl get Pods` 

**Scale Your Application**

* An application can either be scaled up or down depending on the situation in two ways. Using the first method, you can edit the configuration file we created earlier in this exercise by changing the replicas property value to a higher number to scale up or a lower number to scale down.To scale up, edit the manifest file and change the replicas value from 2 to 5. Save and exit the vi terminal and then run the command - `kubectl replace -f replicaset-app.yaml`
* To get the status run the command - `kubectl get replicaset my-replicaset`
* Next, you can check the status of the Pods running in the ReplicaSet `kubectl get Pods`
* As seen in the ReplicaSet and Pod statuses, there are 5 Replicas running and ready for use, as well as 5 Pods. To scale down, change the value of the replicas in the manifest file from 5 to 1, then run the command `kubectl replace -f replicaset-app.yaml` 
* Check the status of the ReplicaSet `kubectl get replicaset my-replicaset`
* Check the status of the Pod `kubectl get Pods`
* You can see that the number of Replica and the running instance of a Pod are the same as the number specified in the manifest file replicas field.
* An application can also be scaled up or down using the command line which is the second method.

> `kubectl scale - -replicas=5 -f replicaset-app.yaml `  ## Scale up

> `kubectl scale - -replicas=1 -f replicaset-app.yaml`  ## Scale down
OR

> `kubectl scale - -replicas= 5 replicaset <replicaset name>`   ## Scale up

> `kubectl scale - -replicas= 1 replicaset <replicaset name> `  ## Scale down

* Kubernetes ReplicaSet can automate application lifecycle management for efficiency






