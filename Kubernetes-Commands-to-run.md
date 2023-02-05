**To Install Minikube** refer - https://minikube.sigs.k8s.io/docs/start/ 
( For now, this is all ready installed on your VM ) 

# List of commands we will be using for the class - 

### Open a terminal on your VM and run the following commands:

> To know the version of kubectl use syntax - `kubectl version --client`

> To know the version of Minikube use syntax - `minikube version` 

### Once kubectl & minikube are installed & verified, you can now easily play around. 

> Just enter `minikube` in your terminal and you'll see a list of commands and options available to you.

### Start your Cluster

> Simply start a new cluster using the docker driver by using the syntax - `minikube start --driver=docker` command. 

_Hold on for some time and let minikube do its job._ 

### Status of your Cluster 

> To check the status of your Cluster use syntax - `minikube status`

### Understanding kubectl

> _Once your minikube cluster is up, you can start interacting with it using kubectl. kubectl actually gets installed as a dependency when we install minikube - which you can see while installing dependencies for miniKube- "kubernetes cli" is actually kubectl. One simple way of using kubectl is to get used to the command format. We've simplified this for you - _

**`kubectl <what action do you want to do> <on which resource> -n <your namespace> <other options> <name of the resource>`**

### kubectl Namespace
 
Namespaces provides a mechanism for isolating groups of resources within a single cluster. If you don't provide a namespace, kubectl will directly use the "default" namespace for you.

> To create a namespace use syntax - `kubectl create namespace my-namespace`

> To list the current namespaces in a cluster using `kubectl get namespace`

> To get pods across all namespaces use syntax - `kubectl get pods -A`

### Managing your Cluster 

> To pause Kubernetes without impacting deployed applications use syntax - `minikube pause`

> To Unpause a paused instance use syntax - `minikube unpause`

> To Halt the cluster use syntax - `minikube stop` 

> To Delete all of the minikube clusters use syantax `minikube delete --all`






