## Understand the pod.yaml file that's used to create a Pod

Kubernetes supports YAML files to create and configure Kubernetes objects. In YAML files, the Indentation plays a very crucial role. So if you give one extra space, then it will fail to execute the commands.

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```

**In the above set of instructions:**
* apiVersion - It is the Kubernetes API server version that needs to be specified. 
* kind - It is Kubernetes object name that we are creating - in this case it is Pod. Other components that you can use as object names are Services, ConfigMap, Secrets, Deployment, etc. The apiVersion value will change based on the kind value.
* metadata - It is a dictionary. We will name the Pod here. And we can give labels to the application.
* name - Here you can specify the Pod name.
* labels - It is used to add labels to the Pod. These labels are used to filter the Pods. We can give any custom key value pair under the labels section. Here I used the following labels “app, type”.
* spec - It is specification section. Containers are specified here.
* containers - It is an array. We can specify any Docker image that we want the container to host.
* name - Name for the container.
* image - Docker image name.