# Kubernetes Create a Pod 

The following is an example of a Pod which consists of a container running the image nginx:1.17.0 

* Open your terminal and create a directory using the syntax `mkdir testpod`
* Get inside the directory using the syntax `cd testpod`
* Create a configuration file using the syntax `touch sample-pod.yaml`
* Open the file using an text editor - For example use `vi sample-pod.yaml`
* Add the following set of instructions 

```
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx:1.17.0
```
* Run the command `kubectl apply -f sample-pod.yaml`
* To check the pod creation - use syntax `kubectl get all` or `kubectl get pods` 
* To get more details about the pod using the name - use syntax ` kubectl describe my-nginx-pod`
* To launch the shell inside our pod use syntax - `kubectl exec -it nginx -- /bin/bash`
* Once inside the pod, try running `curl http://localhost`
* But, it will complain that curl is not found.
* So, we can use apt – advanced package tool – which is used to install/remove software on various Linux distros
* Now, do `apt-get update`
* Then, run `apt-get install curl` [ say yes 'y' for any prompt that comes up ]
* Finally, try to `curl localhost` again
* You should get a response this time w/ the nginx default html!
* Type “exit” to leave the shell