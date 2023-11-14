
---------------------------3----------------------------
[Version Control]

-------------------------------7---------------------------
[Container with Docker]


[6/17] Lesson74 Main Docker Commands
```
docker pull <image name>
docker image
docker run <image name>
docker ps
docker stop <cid>
docker ps -a
docker start <cid>
```

Container Port vs Host Port
- How two containers listening to same port doesn't have conflict ?
- Explain Port Binding.  
` docker run -p<host port>:<container port> -d <image name> `
- Why we need to run the container in detached mode ?

[7/17] lesson75 debugging Docker containers
```
docker exec -it <cid> /bin/bash
docker logs <cid> or docker logs <NAMES>
docker run --name <custom NAMES> <Image>
```

[8/17] Lesson76 Demo project - Demo Overview  
Workflow with Docker

[9/17] Lesson77 Demo project - Developing with Docker  
Frontend,Backend - Local ; MongoDB,Mongo Express - Container  
Docker Network  
Running MongoDB container  
Environment Variables in MongoDB Container  
`docker run -p<Host port>:<container port> -d -e MONGO_INITDB_ROOT_USERNAME=<USERNAME> -e MONGO_INITDB_ROOT_PASSWORD=<PASS> --net <docker network> <image>`  
`docker run -d \`  
Running Mongo Express container  
Environment Variables in Mongo Express Container  
```
docker logs -f
docker logs | tail
```

[10/17] Demo Project - Running multiple Services with Docker Compose
Docker Compose yaml for mongodb
Docker Compose yaml for mongoexpress
- Docker Compose takes care of creating a common network for MongoDB and Mongo-Express in a single compose file?
- Will it create a common network for multiple compose files in a single command?
- For which cases docker compose creates a common network?
```
version: '3'
services:
	mongodb:
		image: mongo
		ports:
		- 27017:27017
		environment:
		- MONGO_INITDB_ROOT_USERNAME=admin
		- MONGO_INITDB_ROOT_PASSWORD=password
	mongo-express:
		image: mongo-express
		ports:
		- 8080:8081
		environment:
		- ME_CONFIG_MONGODB_ADMINUSERNAME=admin
		- ME_CONFIG_MONGODB_ADMINPASSWORD=password
		- ME_CONFIG_MONGODB_SERVER=mongodb
```
docker-compose -f <compose file> up
docker-compose -f <compose file> down
how to keep the network created by docker-compose on docker-compose down command?
Other ways around to do so?

[11/17] Lesson79 Demo Project - Buildong Image with Dockerfile
What is a Dockerfile?
Explain the commands in Dockerfile, FROM, ENV, RUN, COPY, CMD, and others.
What are the environment variables are available for dockerfile?
Why is it prefereble not to provide user and password in it ?
What is the difference in RUN and CMD?
Most commonly used run commands in dockerfile?
Comand to build docker image?
Difference between dockerfile and image?
How to verify the created container image?
Commands to delete docker image?
On pushing a docker image to registry does it pushes all the required files to it?

[12/17] Lesson80 Docker Registry on AWS
Docker private repository
Registry options
build & tag an image
docker login
docker push

[13/17] Lesson81 Demo Project - Deploy the Application
Image from a private repository AWS ECR
Deploy multiple containers
Deployment Server

[14/17] Lesson82 Docker Volumes
When do we need Docker Volumes?
What is Docker Volumes
3 Volume Types
why prefer named volumes over any type of volumes in production in docker ?

[15/17] Lesson83 Demo project - Docker Volumes
Implement Docker Volumes in docker-compose file for mongoDB.

[16/17] Lesson84 Push/Pull Nexus Repository

[17/17] Lesson84 Run Nexus as Docker Container
why ?


[Advanced]
Docker Swarm vs kubernetes
Docker service
Overlay networks
Load Balancing in Docker
Docker secrets
How do I pass arguments to a container at runtime?
Why do I need to pass arguments to a container at runtime?

------------------------8---------------------------
[Build Automation - CICD with Jenkins]

[1/18] Overview

[2/18] 

------------------------9---------------------------
[AWS Services]

-----------------------10--------------------------
[Container Orchestration with Kubernetes]

[1/26] Lesson118 Overview
Introduction to K8s
Main K8s Components
Minikube & Kubectl - Local Setup
Main Kubectl commands
YAML Configuration File
Organize components - Namespaces
Configure connectivity - Services
Make App available from outside - Ingress
Persist data - Volumes
ConfigMap & Secrets as Volume Types
Deploy stateful Apps - StateefulSet
Package Manager of K8s - Helm
Extending the K8s API - Operator
Microservices in Kubernetes
Production and Security best practices
Authorization in Kubernetes - Role Based Access Control(RBAC)

[2/26] Lesson119 Introduction to K8s
What features do orchestration tools offer ?
- High Availability / No Downtime
- Scalability / High Performance
- Disaster recovery / Backup and Restor

[3/26] Lesson120 Main k8s Components
Nodes & Pods
Services & Ingress
Configmap & Secret
Volumes
Deployment & StatefulSet

[4/26] Lesson121 K8s Architecture
Master - Node
Node Processes
Worker machine in K8s cluster
- Container Runtime
- Kubelet
- Kube Proxy
Master Node in K8s
- Api Server
- Scheduler
- Controller Manager
- ETCD

[5/26] Lesson122 Minikube & Kubectl - Local Setup
Minikube - 1 node K8s Cluster
Kubectl  - Command line tool for k8s cluster

[6/26] Lesson123 K8s CLI-Main Kubectl Commands
CRUD commands :
kubectl create / edit / delete deployment [name]
Status of different K8s components :
kubectl get node/pod/services/replicaset/deployment
Debugging pods :
kubectl logs [name]
kubectl exec -it [name] --bin/bash

kubectl create deployment NAME --image=image [--dry-run] [options]

Describe kubernetes replicaset in between pod and deployment.

deployment->Replicaset->Pod->Container

create deployment
- kubectl aply -f [deployment config file name]

### [7/26] lesson124 Yaml Configfile
3 Parts of K8s Configuration YAML
- Matadata
- Specification
- Status (Automatically generated and added by K8s)

To get more infos about pods
- Kubectl get pod -o wide

Get deployment config from ETCD
- Kubectl get deployment [name] -o yaml

delete deployment
- kubectl delete -f [Configfile]

### [8/26] lesson125 Demo project : Complete App Setup with K8s Components
Wtch it later

### [9/26] Lesson126 K8s Namespaces
- What is K8s Namespace?  
: Organize resources in Namespaces  
: Virtual cluster inside a Cluster
- What are the use cases?
- How Namespaces work and how to use it?

- List all namespaces
	- ` Kubectl get Namespace `  

- 4 Namespace per Default
	- Default
	- kube-node-lease
	- kube-public
	- kube-system
	- kubernetes-dashboard  (only with Minikube)

- default 
	- resources I create are located here (without namespace)   

	Create namespace
	- ` Kubectl create namespace [name] `  

	Create namespace with a Configuration file.

- Kube-node-lease
	- heartbeats of Nodes
	- each node has associated lease object in Namespace
	- determines the Availability of a node

- kube-public
	- publicly accessible data
	- A Configmap, which contains cluster information
	- ` kubectl cluster-info `

- kube-system
	- Do NOT create or modify in kube-system
	- System processes
	- Master and kubectl processes

- Divide resources   
	- 1 Divide teams by Name 
	- 2 Divide Staging and Development
	- 3 Divide Blue/Green deployment application common resources
	- 4 Access and resources limits on Namespaces 

Characteristics of Namespaces
- You can't access most of the resources from another Namespace
	- Each NS must define own Configmap/Secret 
- Access service in another Namespace
	- DB/elastic Search/Nginx
- Components, which can't be created within a Namespace
	- Live globally in a cluster
	- can't isolate
	- Volume/node
	- ` Kebectl api-resources --namespaced=false `

Create component in a Namespace
- ` kubectl apply -f [deployment yaml] --namespace=[name] `

Change active namespace
- with kubens [kubectlx]
- ` kubens [namespace name] `

### [10/26] Lesson127 Kubernetes Services
- What is a kubernetes Service and when we need it?
	- Each Pod has its own IP address
		- Pods are ephemeral - are destroyed frequently!
	- Services 
		- Has stable IP address
		- Loadbalancing
		- Good for Loose coupling 
		- Within & outside cluster
- Diferent Service types in kubernetes
	- ClusterIP Services
		- Default type - takes when not specified
		- Multi-Port services
			<!-- - Service open to two different applications -->
	- Headless Services
		<!-- - Client wants to communicate  -->
	- NodePort Services
		- extension of ClusterIP
		- 30000 - 32767
		- Not Secure
	- LoadBalancer Services
		- Extension of NodePort
- Differences between them and when to use which one 

### [11/26] Lesson128 Ingress- Connecting to Applications outside   
- External Service vs Ingress 
- How to Configure ingress in your Cluster?
	- Ingress Controller Pod
		- evaluates all the rules
		- manages redirections
		- entrypoint to the cluster
		- many third-party implementation
- Configure ingress in minikube
	- Ingress controller in minikube
		- ` minikube addons enable ingress `
		- ` kubectl get pod -n kube-system `
	- Create Ingress rule
		- ` kubectl apply -f dashboard-ingress.yml `
		- ` kubectl get ingress -n kubernetes-dashboard `
		- ` kubectl get ingress -n kubernetes-dashboard --watch `
	- Local Domain to ipadress resolution
		- ` sudo vim /etc/hosts `
		- Put `<IpAddress> <URL>`

- Ingress Default Backend
	- ` Kubectl describe ingress [name of ingress] -n [namespace] `
	- Configure the preexisting default backend 

- More Use cases
	- Multiple paths for same host
	- Multiple Domains/Subdomain
- Configuring TLS Certificate - https://
	

### [12/26] lesson129 Persisting Data with Volumes
- Persistant Volume
- Persistant Volume Claim
- Storage Class

- Need
	- Storage that doesn't depend on the pod lifecycle
	- Storage must be available on all nodes
	- Storage needs to survive even if cluster crashes

- Persistant Volume YAML Example
- ***Persistant volumes*** are not namespaced. They are available to the whole cluster. ***Persistance volume claims*** are namespaced.
- Local vs Remote Volume Types
	- Local volume types violate 2. and 3. requirement for data persistance.
- Who creates Persistent Volumes & when ?
	- Persistent volume claim
- Level of Volume Abstractions
- ConfigMap & Secret
- Storage Class
	- Provisions Persistent volumes dynamically.
	- Initiated by PVC

### [13/26] lesson130 ConfigMap & Secret Volume Types
- How to create ConfigMap & secret as K8s Volumes ?
- When to use ConfigMap & Secret Volumes ?
- ConfigMap & secret for mounting files
- ConfigMap & Secret are Local Volume Types

### [14/26] lesson131 Statefulset
- Statefull applications get deployed using StatefulSet not Deployment
- Deployment vs StatefulSet
	- StatefulSet provides Pod Identity, Created from *Same spesification*, but *not interchengeable* ! Persistent identifier across any re-scheduling. Fixed order name (identifier). Fixed individual DNS name.
- Why identity of pod is important ?
	- Scaling database applications
		- Master-Slave , data synchronisation

### [15/26] lesson132 Managed K8s Services
- Managed vs Unmanaged K8s Services

### [16/26] lesson133 Helm Package Manager
- What is helm ?
	- K8s Package manager
	- Templating Engine
	- Release Management
- Helm Chart Structure
- Values Injection
	- ` helm install --values=my-values.yaml [chartname] `
	- ` helm install --set [parameter]=[value] `
- Tiller
	- Too much power in K8s Cluster so removed in V3

### [17/26] lesson134 Helm & K8s Demo
- Browser -> ngnix-Ingress -> mongo-express -> mongodb-pod -> pv
- Set as environment variables
	- ` export KUBECONFIG=test-kubeconfig.yaml `
- Show remote K8s Cluster
	- ` kubectl get node `
- Create MongoDB pods
	- ` helm repo add bitnami https://charts.bitnami.com/bitnami `
	- ` helm search repo bitnami/mongo `
	- ` helm install [our name] --values [path to file] [chart name]`
- check everything got created
	- ` kubectl get all `
- Deploy MongoExpress
	- Create deployment file for pod & service  & go
- Deploy Ingress Controller
	- ` helm repo add stable https://kubernetes-charts.storage.googleapis.com/ `
	- ` helm install nginx-ingress stable/nginx-ingress --set controller.publishService.enabled=true `
- create ingress rule
	- ` kubectl apply -f test-ingress.yaml `
- Scaledown 
	- ` kubectl scale --replicas=0 statefulset/mongodb `
- Uninstall charts
	- ` helm uninstall mongodb `

### [18/26] lesson135 Deploy App from Private Docker Registry
- `Commit`->`Triggers CI build`->`Jenkins packages application`->`pushed to registry`->`private registry`
- Steps to pull image from private registry
	- Create Secret component
		- Credentials
	- Configure Deployment/Pod
		- `imagePullSecrets`

- create `config.json` file for secret to Docker Login
- `aws ecr get-login`
- `docker login -u [....] -p [....] -e [none] [registry URL]`
- ` cat .docker/config.json `
- ` minikube ssh `
- ` scp -i $(minikube ssh-key) docker@(minikube ip): .docker/config.json .docker/config.json `
- ` cat .docker/config.json | base64 `
- ` kubectl create secret generic my-registry-key --from-file=.dockerconfigjson=.docker/config.json --type=kubernetes.io/dockerconfigjson `
- ` kubectl get secret `
- ` kuibectl create secret docker-registry my-registry-key-two --docker-server=[registry URL] --docker-username=[name] --docker-password=[password] `
- Add the below code to the deployment file for authentication
```yaml
spec: #it will be added under spec
	imagePullSecrets:
	-	name: my-registry-key
```
- Secret has to be in same namespace as deployment

### [19/26] lesson136 K8s Operator
- Use
	- Stateful Application
- What it is ?
	- Custom Control loop mechanism
	- Makes use of CRD's (Custom Resource Definitions)
	- Has domain/app-specific knowledge 
- Operatorhub.io
- Operator SDK

### [20/26] lesson137 Operator & Helm Demo
- Setup Prometheus Monitoring in K8s
- Prometheus Architecture
	- *Prometheus Server* processes & stores metrics data
	- *Alertmanager*: Send alerts
	- *Prometheus Web UI* visualize the scraped data in UI or Grafana
- Ways to deploy Prometheus (in increasing eficiency order)
	- Create and execute the config.yaml file in dpendency order
	- Deploy them through operator
	- Use helm chart to deploy operator to deploy prometheus
- setup with HelmChart
	- ` helm install prometheus stable/prometheus-operator `
	- Hula 😃 Everything will be created
- Access Grafana

### [21/26] lesson138 Secure your Cluster - Authorization with RBAC
- How Authentication & Authorization works in K8s
- How to configure users, groups & their permissions
- Autorization with RBAC (Role Based Access Control)
- Which K8s resources to use to define permissions in the cluster
- Components
	- Role
	- RoleBinding
	- ClusterRole
	- ServiceAccount
- Creating and Viewing RBAC Resources
- ` kubectl auth can-i create deployments --namespace dev `
- Authentication vs Authorization in K8s

### [22/26] lesson139 Microservices in K8s P1 - Introduction to Mocroservices
- Main Benifits of Microservices Applications
- What you need to know to deploy Microservice Applications in a K8s Cluster
- Service Mesh Architecture
	- Isito
<!-- - Information U need from Developers as Devops Engineer
	- Which microservices u need to deploy?
	- Which microservice is talking to which microservice?
	- How R they communicating?
	- Which database are they using? + 3rd party services
	- On which port each microservice run? -->

### [23/26] lesson140 Microservices in K8s P2 - Demo project Deploy Microservices Application
- *Load Generator* Test load of application
- *emptyDir* for Redis
```yml
volumes:
-	name: redis-data
	emptyDir: {}
```
- ` chmod 600 ~/Downloads/online-shop-kubeconfig.yaml`
- 600 - read write only by user
- 400 - read only , only by user
- ` export KUBECONFIG=~/path-to-K8s-service-provider-config-file `
- ` kubectl get node `
- ` kubectl apply -f config.yaml `


------------------------12--------------------------  
[Infrastructure as Code with Terraform]

------------------------14--------------------------
[Automation with Python]

------------------------16-------------------------
[Monitoring with Prometheus]







































