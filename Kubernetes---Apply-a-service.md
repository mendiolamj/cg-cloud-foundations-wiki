# Apply a service locally and connect it to our nginx POD 

Our goal here is to apply a service locally and connect it to our nginx POD that we previously created.  That way, we can access nginx through our browser (using Minikube -ip)  Remember – pods are not directly accessible outside of K8s cluster, we must expose them through a service!

* Open your terminal on your VM and get inside the directory k8s - use syntax `cd k8s` .
* create a configuration file and name it as "service.yaml" - use syntax `vi service.yaml`
* Copy the below instructions - while maintaining Indentation as seen below:
```
kind: Service
apiVersion: v1
metadata:
  name: my-nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    nodePort: 30001
  type: NodePort
``` 

* Run the command to create a service -  `kubectl apply –f service.yaml`
* So, our service is exposing our pod, which in turn is running the nginx Docker container.
* Run the command `kubectl get services` and check the TYPE, CLUSTER-IP and the PORT(S) of the service you created. 
* Note: TYPE = NodePort, Service Port inside the cluster is 80 which is mapped to 30001 - where it is now exposed! 
* To access the application from the browser - using the NodePort service - run a command `minikube service my-nginx-service`
* You will now be exposed to the IP of the Minikube with the nodePort exposed!
* Curl the IP address to the exposed IP or open the same using a browser! 


