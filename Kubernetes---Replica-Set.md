# Create a Kubernetes Replica Set 

* Create a replicaset configuration file use syntax `vi replicaset.yaml`
* Copy the below instructions - while maintaining Indentation as seen below: 

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```


* Run the command - `kubectl apply -f replicaset.yaml`
* You can see the pods created by the replicaset if you do a `kubectl get all`
* Try deleting a pod and see what happens – it should self heal and bring it back up (`kubectl delete pod/pod-name`)
* Look at desired, current and ready states! Run `kubectl get Pods` and You can then get the current ReplicaSets deployed using `kubectl get rs`
* Your service should still be running
* Note that this replicaset is still attaching to our service through the label selector
* so if service is still running you should be able to see the nginx response in the browser by going to http://localhost:30001/
* Try deleting the service, do you still see the nginx homepage?
* Everything under template is specific to the pods its creating
* Note that we don’t explicitly use the word pod in the replicaset.yaml file, it’s implied!
