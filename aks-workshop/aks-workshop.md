# AKS WORKSHOP
Website Components:
- Web frontend
- Document database that stores captured data
- RESTful API

The API allows the Web frontend to communicate with the database. Since the leading website stores the sensitive data and additional requirement is needed to protect the site with https certificate.

The company most used Kubernetes as compute platform. 




### Step-1

### Parameters:

REGION_NAME=eastus

RESOURCE_GROUP=aksworkshop

SUBNET_NAME=aks-subnet

VNET_NAME=aks-vnet


AKS_CLUSTER_NAME=aksworkshop-9865

ACR_NAME=acr28364


### Create Resource Group

az group create \
  --name $RESOURCE_GROUP \
  --location $REGION_NAME


### Networking

We have two networking models.
	1. Kubenet networking
	2. Azure CNI (Container Networking Interface)

1. Kubenet networing is the default networking model in Kubernetes. With Kubenet networking, nodes get assigned an IP address from the Azure Virtual Network's Subnet. And Pods would recieve an IP from logically different address space to the Azure Virtual Network's subnet of the nodes. Your Network Address Translation which is NAT is then configured so that the pods can reach resources on the Azure VNet. The source IP address of the trafic is translated to the node's primary IP address and then configured on the nodes. And one thing to note here is that the pods receive an IP address that's hidden behind the node IP.  

And if you think what is container networking which is CNI

2. With CNI, the AKS cluster is connected to existing virtual network resources and configurations. In this networking model every pod's IP address from the subnet and can be accessed directly and these IP addresses must be unique across the network space and will be calculated in advance. Some of the features you will use, require you to deploy the AKS cluster by using the CNI which is Container Networking Interface.

 
First I will create VNet and Subnet. And Pods deployed in the cluster, will be assigned an IP from this subnet.



### Creating VNet and Subnet

az network vnet create \
  --resource-group $RESOURCE_GROUP \
  --location $REGION_NAME \
  --name $VNET_NAME \
  --address-prefixes 10.0.0.0/8 \
  --subnet-name $SUBNET_NAME \
  --subnet-prefixes 10.240.0.0/16


### Storing Subnet id

SUBNET_ID=$(az network vnet subnet show \
  --resource-group $RESOURCE_GROUP \
  --vnet-name $VNET_NAME \
  --name $SUBNET_NAME \
  --query id -o tsv)


Then I will create AKS cluster. There are two values, we need to know before running the "az aks create" command to create AKS cluster. 
1. The version of the latest. The Kubernetes version available in the region. So to get the latest non-preview kubernetes version. I should use "az aks get version" command.

VERSION=$(az aks get-versions \
  --location $REGION_NAME \
  --query 'orchestrators[?!isPreview] | [-1].orchestratorVersion' \
  --output tsv)


2. AKS cluster name should be unique

AKS_CLUSTER_NAME=aksworkshop-$RANDOM


### Creating AKS Cluster
az aks create \
  --resource-group $RESOURCE_GROUP \
  --name $AKS_CLUSTER_NAME \
  --vm-set-type VirtualMachineScaleSets \
  --node-count 2 \
  --load-balancer-sku standard \
  --location $REGION_NAME \
  --kubernetes-version $VERSION \
  --network-plugin azure \
  --vnet-subnet-id $SUBNET_ID \
  --service-cidr 10.2.0.0/24 \
  --dns-service-ip 10.2.0.10 \
  --docker-bridge-address 172.17.0.1/16 \
  --generate-ssh-keys
  

I have specifically specified that the VM set type should be VMSS and the VM Scale sets enable you to switch on the cluster auto scaler when needed.

We are specifying the creation of AKS cluster by using the CNI plug-in. The networks wherein the pods get ip address from the vNet that we are creating 

And the service cidr that we use this address range is the set of virtual IPs that kubernetes assigns to internal services in the cluster. The range must not be within the virtual network ip address range of the cluster. It should be different from the subnet created for the pods.

And the dns-service-ip that we use the ip address is for the clusters dns service. This address must be within the kubernetes service address range. Please don't use the first IP address in the address range such as 0.1 . The first address in the subnet range is used for the kubernetes.default.svc.cluster.localaddress . 


And the docker bridge address. The docker-bridge-address represtents the default docker-bridge-address present in all docker installations . AKS clusters or the pods themselves don't use docker bridge. However we have to set this address to continue supporting scenarios such as docker build(t) within the aks cluster. It's required to select a classless enter domain routing which is C IDEA for the docker bridge network address. If you don't set a cicr, docker choose a subnet automatically. This subnet could conflict with other CIDRs. So if I get the or If I want to test the connectivity by using kubectl,

kubectl is the main kubernetes command line. We used to intract with the cluster and is available in Cloudshell.

So I'm going to use "az aks get-credentials" to connect AKS cluster. I'm going to hit below command

### To connect with AKS cluster
az aks get-credentials \
  --resource-group $RESOURCE_GROUP \
  --name $AKS_CLUSTER_NAME


### Getting nodes status 
kubectl get nodes

Now I have to create a kubernetes namespace for the application. So this company's (website) want to deploy several apps from other teams in the deployed AKS cluster as well.

So instead of running multiple clusters the company wants to use the kubernetes feature that let you logically isolate team and workloads in the same cluster so I can create different namespaces and deploy different applications. The goal is to provide the least number of previleges scope to the resource each team needs. So what is a namespace.

A namespace in kubernetes creates a logical isolation boundary. Names of resources must be unique within a namespace but not across namespaces. 

If we don't specify the namespace, when we work with kubernetes resources, the default namespace is implied.

So let me create a namespace. If I go and do "kubectl get namespaces" for listing namespaces.  
### Creating namespace
### Logically isolated

kubectl get namespaces

kubectl create namespace <desired namespace name>
kubectl create namespace ratingsapp

kubectl get namespaces




## Step-2 :: How to create a private container registry

Let's deploy a container registry for the environment. Container Registry name also should be unique.

ACR_NAME=acr$RANDOM

az acr create \
  --resource-group $RESOURCE_GROUP \
  --location $REGION_NAME \
  --name $ACR_NAME \
  --sku standard
  
  
  
### Create docker images and push to ACR.


### Build ratings-api image and push to ACR. (Written in node.js using Express)

git clone https://github.com/MicrosoftDocs/mslearn-aks-workshop-ratings-api.git

cd mslearn-aks-workshop-ratings-api

az acr build \
  --resource-group $RESOURCE_GROUP \
  --location $REGION_NAME \
  --image ratings-api:v1 .

(or) new

az acr build -t ratings-api:v1 -r $ACR_NAME .


### Build ratings-web image and push to ACR (Written in node.js using Vue)

git clone https://github.com/MicrosoftDocs/mslearn-aks-workshop-ratings-web.git

cd mslearn-aks-workshop-ratings-website

az acr build \
  --resource-group $RESOURCE_GROUP \
  --location $REGION_NAME \
  --image ratings-web:v1 .

(or) new

az acr build -t ratings-web:v1 -r $ACR_NAME .

### To list the docker images in the ACR
az acr repository list --name $ACR_NAME --output table



Now I will configure the AKS cluster to authenticate to the ACR. So we need to setup the authentication between the container registry and kubernetes cluster to allow communication between these two services. So I'm going to integrate the container registry by replying the AKS cluster name value and the ACR value.

I will use "az aks update" command to authenticate my azure container registry in which we have two docker images that we created using the github repo.


az aks update \
  --name $AKS_CLUSTER_NAME \
  --resource-group $RESOURCE_GROUP \
  --attach-acr $ACR_NAME
  
AAD propagation will add with above command.


Step-3 :: How to Deploy MongoDB to AKS cluster using Helm
------------------------------------------------------------

Helm is an application package manager for Kubernetes. It offers a way to easily deploy applications and services using charts. 

The helm client is already installed in the azure Cloudshell and can be run with helm command. 

Helm wolud provide a standard repository of charts for many diffrent software packages. Helm has a chart for mongodb that is part of the official helm bitnami chart repository.

We'll have to configure the client to use this table repository by running the "helm repo add" command.
So I'm going to run this from the helm repository like we have to get bitnami, like we have the github also. 

```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
```
The above command will work like git clone or forke the repo.

```
helm search repo bitnami 
``` 
Helm chart is a collection of files that would describe a related set of kubernetes resources. I can use single chart to deploy something simple like a memcached pod or something complex like a full web stack with http servers, database and caches. Helm chart s are stored in helm chart repository. The official chart repository is maintained on github.

Now we are ready to install the mongodb instance.

```
helm install ratings bitnami/mongodb --namespace ratingsapp --set auth.username=vamsee,auth.password=vamsee,auth.database=ratingsdb
```

The "helm install" command is a powerful command with many capabilities. Keep in mind that we can easily remove release by running the "helm uninstall" command.

I'm gonna create a kubernetes to hold the mongodb details. In the previous step, we installed mongodb using helm with the specified username, password and database name.


Now I will store these details in kubernetes secret. This step ensures that we don't leak secrets by hard coding them into configuration files.

Kubernetes has a concept of secrets. Secrets let you store and manage sensitive information such as passwords. Putting this information in a secret is safer and more flexible than hard coding it in a Pod definition or a container image.

The ratings-api expects to find the connection details to the mongodb database in the form of like mongodb then the username then the password then the endpoint name on the port number with the database. 

So I'm going to replace username, password and the endpoint.

```
kubectl create secret generic mongosecret \
  --namespace ratingsapp \ 
  --from-literal=MONGOCONNECTION="mongodb://vamsee:vamsee@ratings-mongodb.ratingsapp:27017/ratingsdb"
```
(or)  
```
kubectl create secret generic mongosecret --namespace ratingsapp --from-literal=MONGOCONNECTION="mongodb://vamsee:vamsee@ratings-mongodb.ratingsapp:27017/ratingsdb"
```
I don't have to mention my username and password in the configuration files I do not have to hard code them. I can retreive them from the Kubernetes secrets. 

```
kubectl describe secret mongosecret --namespace ratingsapp
```


Now I have AKS cluster with a configured mongodb database in a namespace called ratingsapp. In this namespace, we'll find the following resources. You would have a deployment ratings mongodb. A deployment represents one or more identical pods managed by the Kubernetes deployment controller. This type deployment defines the number of replicas or the pods to create for mongodb. The Kubernetes scheduler ensure that if pods or nodes encounter problems additional pods are scheduled on healthy nodes and you also see a pod of the name ratings mongodb. The Kubernetes uses pods to run an instance of mongodb. Inside pod you see the containers and I have a service which is called as ratings mongodb to simplify the network configuration.

Kubernetes uses services to group a set of pods and provide network connectivity logically. Connectivity to the mongodb database is exposed via this service through the DNS name. so the name of my service would be ratings mongodb.ratingsapp.svc.cluster.com and I have a secret which is called as a mongosecret and Kubernetes secret is used to inject sensitive data into pods such as access credential or keys. This secret holds the mongodb connection details. We'll use it to configure the api to communicate with mongodb, just to make sure that it is authenticated to access the database.


So what we did? we configured helm stable repository, then used a helm chart to deploy mongodb to the cluster. We then created Kubernetes to hold database credentials.

No we will deploy ratings-api to the AKS cluster.

Step-4 :: How to deploy API to the Azure Kubernetes Service(AKS) cluster
------------------------------------------------------------------------

We will see how to deploy API to the Azure Kubernetes Service cluster. This is Step-4, in Step-3 I was able to configure helm stable repository using helm chart to deploy mongodb to the AKS cluster. In this Step-4, we will deploy the API to the AKS cluster.

We will create a Kubernetes deployment for the Restful API that will be a connection between my mongodb instance and the web frontend that is my ratings-api and we will create a kubernetes service to expose the RESTful API over the network.


Before logging on to the portal, if you would just see the architecture. 
pic: ratings-api.png

So I have Kubernetes Service Cluster, I have my ratings API that I will deploy and this deployment will give you a subway to provide declarative updates for pods. We will describe the desired state of the workload in a deployment manifest file and use kubectl to submit the manifest to the deployment controller in AKS plane(cluster) and then the deployment controller in turn actions the desired state of the defined workload. 
For example, It would deploy a new pod, it will increase the pod count or decrease the pod count.


I have to create a manifest file for the Kubernetes deployment called ratings-api, then the yaml file would be rating-api-deployment.yml

The azure cloudshell includes an integrated file editor as well. The Cloudshell editor supports features such as language highlighting, command palette and file explorer.

We can just for a simplified creation in editor you can launch the editor by running "code filename"


###
code rating-api-deployment.yaml
---------------------------------
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ratings-api
spec:
  selector:
    matchLabels:
	  app: ratings-api
  template:
    metadata:
	  labels:
	    app: ratings-api  # the label for the pods and deployements
	spec:
	  containers:
	  - name: ratings-api
	    image: <acrname>.azurecr.io/ratings-api:v1   # IMPORTANT: update with your repository
		imagePullPolicy: Always
		ports:
		- containerPort: 3000  # the application listens to this port
		env:
		- name: MONGODB_URI  # the application expects to find the MongoDB connection details in this environment variable.
		  valueFrom:
		    secretKeyRef:
			  name: mongosecret  # the name of the Kubernetes secret containing the data
			  key: MONGOCONNECTION  # the key inside the Kubernetes secret containing the data
	    resources:
		  requests: # minimum resources required
		    cpu: 250m
			memory: 64Mi
	      limits: # maximum resources allocated
		    cpu: 500m
			memory: 256Mi
	    readinessProbe:  # is the container ready to receive traffic?
		  httpGet:
		    port: 3000
			path: /healthz
		livenessProbe:  # is the container healthy?
		  httpGet:
		    port: 3000
			path: /healthz
```
-------------------------------

```
kubectl apply \
  --namespace ratingsapp \
  -f rating-api-deployment.yaml
```  
```
kubectl get pods \
  --namespace ratingsapp \
  -l app=ratings-api -w
```
-w --> To watch
Ctrl+C --> To stop watch

```
kubectl get pods --namespace ratingsapp
```
```
kubectl get deployment ratings-api --namespace ratingsapp
```


Now I am going to create a Kubernetes service for the ratings-api service. 

A service is a Kubernetes object that would provides a stable networking for pods by exposing as a network service. We use Kubernetes service to enable communication nodes, pods and users of the application both internal and external to the cluster service just like  a node or pod gets an IP address assigned by Kubernetes when we create them.

Services are also assigned a dns name based on the service name and a tcp port. A ClusterIP would allow us to expose a Kubernetes service on an internal IP in the cluster and this type would only make the service only reachable from within the cluster.

So our next step is to simplify the network configuration for the application workload. We'll use the Kubernetes service to group our pods and provide network connectivity.

I'm going to create a manifest file for the Kubernetes service called yaml and then would apply that configuration

###
code ratings-api-service.yaml
-----------------------------
```
apiVersion: v1
kind: Service
metadata:
  name: ratings-api
spec:
  selector:
    app: ratings-api
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: ClusterIP
```
---------------------------


Now if you would see the selector section, what does it say? the selector determines the set of pods targeted by a service in the following file Kubernetes load balance the traffic to pods that have the label as ratings-api. This label was define when we cretaed the deplyment. The controller for the service continuosly scans for pods that match that label to add then to the load balancer.

If you would see ports section here, this one a service can map an incoming port to targetPort. So incoming port is what the service responds to over the internet and the targetPort is what the pods are configured to listen.

The service is exposed internally within the cluster at port 80 and the load balances the traffic to the ratings-api listening on port 3000.

If you would see, the type is ClusterIP. The service of type ClusterIP creates an internal IP address for use within the cluster. Choosing this value makes the service reachable only from within the cluster and it is the default service type.

So same way I'm gonna save it  and then I'm going tp apply my configuration.


```
kubectl apply --namespace ratingsapp -f ratings-api-service.yaml
```
```
kubectl get service ratings-api --namespace ratingsapp
```

Now the service show an internal ip address which is 10.2.0.43

By default Kubernetes creates a DNS entry that would map to my service local name. So notice how cluster ip comes from the kubernetes service address, we defined ehen we created the cluster. So finally we have validate the endpoints. Services would load balance traffic to the pods through endpoints.

The endpoint has the same name as the service. Now I have to validate that the service points to one endpoint that corresponds to the pod. 

So I'm going to use "kubectl get endpoints" for my ratings-api

```
kubectl get endpoints ratings-api --namespace ratingsapp
```

This is the endpoint on which my service is listening and this is also coming from the subnet that I defined when we created a cluster. so we have now creat a deplyment of the ratings-api, it have exposed it to internal service.

The name of the deployment is ratings-api, the api running a replica which reads the mongodb connection details by mounting secret when that we deployed and we have a service called as ratings-api under the ratings-api. This API is exposed internally within the cluster at ratings-api.ratingsapp.svc.cluster.local on the port 80. 

What did we do in this Step-4, we created a kubernetes deployment for the ratings-api by creating a deployment manifest file and the applying it to the cluster.
We also created a kubernetes service for the ratings-api by creating a service manifest file and the applying it to the cluster.
We now have a ratings API endpoind that is available through a ClusterIP over the network.

In the next step, you will also see the similar process to deploy the ratings website, the frontend that would accept the 



Step-5 :: How to deploy FrontEnd API to the Azure Kubernetes Service (AKS) Cluster
----------------------------------------------------------------------------------


We will see how to deploy the frontend API to the Azure Kubernetes Cluster. If you remember that we are working on a project wherein a company has deployed a ratings website which would consist of the web frontend, a document database and restful e-writings api that would allow the frontend to communicate with the database. 

In previous step, we have deployed the ratings-api. Now we will continue the deployment and deploy the ratings web frontend. This web frontend is a node.js application. If you remember and if you recall that we have already created an Azure Container Registry (ACR) instance and we used it to build a docker image of the frontend and store it in a repository. so in this Step-5, we will create a Kubernetes deployment for web frontend and we'll create a Kubernetes service manifest to expose the web frontend and we will test the web frontend

PIC: frontend architecture.png

This is the architecture which we are doing and we are deploying this web frontend bit behind the load balancer with a public IP so that user request can come via internet to this service. The web frontend would communicate with the mongodb through the ratings api which is responsible to extract and to get the data from the mongodb to this web frontend.   

So let me just quickly log on to the portal an I have launched the Cloudshell as well. 

So first of all I will create a blank yaml file

###
code ratings-web-deployment.yaml
--------------------------------
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ratings-web
spec:
  selector:
    matchLabels:
	  app: ratings-web  
  template:
    metadata:
	  labels:
	    app: ratings-web  # the label for the pods and deployments
	spec:
	  containers:
	  - name: ratings-web
	    image: <acrname>.azurecr.io/ratings-web:v1  # IMPORTANT: update with your own repository
		imagePullPolicy: Always
		ports:
		- containerPort: 8080  # the application listens to this port
		env:
		- name: API  # the application expects to connect to the API at this endpoint
		  value: http://ratings-api.ratingsapp.svc.cluster.local
		resources:
		  requests: # minimum resources required
		    cpu: 250m
			memory: 64Mi
	      limits: # maximum resources allocated
		    cpu: 500m
			memory: 512Mi
```
------------------------------


```
kubectl apply \
  --namespace ratingsapp \
  -f ratings-web-deployment.yaml
```
```
kubectl get pods --namespace ratingsapp -l app=ratings-web -w
```
```
kubectl logs <pod name> --namespace ratingsapp
```
```
kubectl logs ratings-web-b44bbcf57-hfvsd --namespace ratingsapp
```
```
kubectl get deployment ratings-web --namespace ratingsapp
```
Let's say the deployement is successful for ratings-web deployment. Now I will have to create a Kubernetes service for the ratings-web frontend. So in this step we'll have to simplify the network configuration for our application workloads we use a Kubernetes service to group the pods and provide network connectivity. I will use the Lubernetes load balancer instead of ClusterIP for this service. A load balancer would allow me or us to expose the kubernetes service on a public IP in the cluster. The type makes the service reachable from the outside from the cluster. So I will create a file like I did earlier.

###
code ratings-web-service.yaml
-----------------------------
```
apiVersion: v1
kind: Service
metadata:
  name: ratings-web
spec:
  selector:
    app: ratings-web
  ports:
  - protocol: TCP
    port: 80
	targetPort: 8080
  type: LoadBalancer
```
-----------------------------


If you would see the selector section here, it sits the set of pods targeted by a service is determined by the selector. The kubernetes load balancer traffic to ports that have labeled
 as ratings-web. The label was defined when we created the deployement. 

```
kubectl apply \
  --namespace ratingsapp  \
  -f ratings-web-service.yaml
```
```
kubectl get service ratings-web  --namespace ratingsapp
```

The service deployemnt is done and the load balancer ClusterIP is this and the external IP on which it will listen the traffic from internet is 40.76.151.178 and the ports are these.


Access the apllication at http://40.76.151.178




Step-6 :: How to configure Kubernetes Ingress controller running NGINX
----------------------------------------------------------------------

We will see how to configure Kubernetes ingress controller running NGINX. In the previous step, we exposed the ratings website and restful api into different ways for allowing access to each instance. The api is exposed via a ratings-api-service using a ClusterIP that would create an internal IP address for use within cluster. And the website, the frontend is exposed via ratings-web-service using a load balancer that creates a public ip address in Azure and assign it to the Azure load balancer. Even though the load balancer exposes the ratings website via publically accessble IP. There are limitations that we have to consider.

Let's assume that the development team decides to extend the project by adding a video upload website. The fans of the smoothies can submit videos of how they are enjoying their smoothies at home. The current ratings website responds at let's say the url is fruitsmoothies.com. When we deploy the new site to respond at fruitsmoothies.com/videos and the ratings site would be fruitsmoothies.com/ratings. 

So if we continue to use the load balancer solution, we need to deploy a separate load balancer on the cluster and map its IP address to a new fully qualified domain name (FQDN) so that would be videos.fruitsmoothies.com. To implement the required url based routing configuration will need to install additional software outside of the cluster. The extra effort is that a kubernetes load balancer service is a layer 4 load balancer. Layer 4 load balancers only deal with routing direct decisions between IP's addresses or the TCP or the UDP ports.

Now Kubernetes provides us with an option to simplify the above configuration by using an Ingress Controller. So in this video, we will deploy a Kubernetes Ingress Conroller. We will reconfigure the ratings-web service to use clusterIP. We will create an ingress resource for the ratings-web service will test the application. So let me show you the architecture now.

There we have it so we are deploying the host frontend to route the http traffic from internet via the public IP then we would have the ingress controller which would route the traffic to the ratings-web frontend.

So we are deploying so in the previous video it was between this frontend and the load balancer. Now we are deploying ingress controller to accept the connection.

So, I will tell you the details while we are doing it. Let me open another tab as well to get the ACR and the AKS information.

We will deploy a Kubernetes Ingress Controller. Kubernetes Ingress Controller is a software that provides Layer 7 load balancer features. These features include reverse proxy conffigurable traffic routing and TLS termination for Kubernetes services.

We install the Ingress Controller and configure it to replace the load balancer. With the Ingress Controller, we can now do all the load balancing, authentication, TLS and URL based routing configuration without the need for extra software outside of the cluster. There are several other options for running Kubernetes Ingress on AKS such as Azure Application Gateway, Ambassador, HA Proxy, Kong Nginx and traffic.

The Ingress Controllers are exposed to the internet by using a Kubernetes service type of Load Balancer. The Ingress Controller watches and implements Kubernetes Ingress resources which create route to application endpoints.

Here we will deploy a basic Kubernetes Ingress Controller by using nginx. Then we will configure the ratings frontend service to use that Ingress for traffic. Nginx Ingress Controller is deployed as any other deployment in Kubernetes. We can either use a deployment manifest file and specify the Nginx Ingress Controller image or we can use an Nginx helm chart. The Nginx helm chart simplifies the deployment configuration required for the Ingress Controller. For example, we don't need to define a configuration mapping or configure a service account for the nginx deployment. 

So we will start by creating a namespace for the Ingress. 
I will do

```
kubectl create namespace ingress
```

It will create a namespace with the name of ingress.

```
Kubectl get namespace
```

Now I can see igress namespace and earlier I was created ratingsapp. Now I will configure helm client to use the stable repository by running the helm repo at command.

```
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```
(or)
```
helm repo add stable https://charts.helm.sh/stable
```

It will add the repo. "stable" has been added. so repo name stable has been added. Next we will install the nginx ingress controller. nginx ingress is a part of stable helm repository which we configured earlier when we installed mongodb. we will install two replicas of the nginx ingress controller with the set controller.replicaCount parameter for added.

We will make sure to schedule the controller only on linux nodes as a windows server node should not run the ingress controller we will specify a node selector by using the set controller.nodeSelector parameter.

```
helm install nginx-ingress stable/nginx-ingress \
    --namespace ingress \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

It did that after the installation is finished, we will see an output like here(show output). Now let's check the public IP of the ingress service. It might take few minutes for the serice to acquire the public IP. Let me just run the command and check.

```
kubectl get service nginx-ingress-controller --namespace ingress -w
```

Now we will reconfigure the ratings-web service to use the ClusterIP. So I'm going to edit ratings-web-service.yaml file.

###
code ratings-web-service.yaml

This is the file that we created earlier. If you would see the type is load balancer and I'm gonna replace the type to ClusterIP.

>>>
kubectl apply

I'm going to use kubectl apply command to apply my configuration and before that I'm going to delete the service so there is a point here that we can't update the point value of type on a deployed service. We have to delete the service and recreate it. I'm going to delete it.

```
kubectl delete service \
   --namespace ratingsapp \
   ratings-web
```

Then I'm going to do the kubectl apply command.

```
kubectl apply \
    --namespace ratingsapp \
    -f ratings-web-service.yaml
```

It took few seconds for the changes to take effect. Now I'm going to create an ingress resource for my ratings-web service and in order for my Kubernetes ingress controller to route request to the ratings-web service.

I will have to have an ingress resource. The ingress resources where we specify the configuration. So each ingress resource will contain one or more ingress rules which specify an optional host a list of paths to evaluate in the request and a backend to route the request to. So I'm gonna edit the file that I did created earlier so I'm gonna do

###
code ratings-web-ingress.yaml
-----------------------------
```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ratings-web-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - host: frontend.<ingress ip>.nip.io   IMPORTANT: update <ingress ip> with the dashed public IP of your ingress, for example frontend.13.68.177.68.nip.ip
    http:
      paths:
      - backend:
          serviceName: ratings-web
          servicePort:80
        path: /
```

In this file I need to update ingress IP value in the host key with the public IP of the ingress that we retrieved earlier. This value would allow you to access the ingress via hostname instead of an IP address. 

```
kubectl apply \
  --namespace ratingsapp \
  -f ratings-web-ingress.yaml
```


And in the next video, we will configure the SSL and TLS on this hostname to have the secure connection.


Step 7: How to Enable SSL and TLS in the Frontend ingress in Azuru Kubernetes Service
------------------------------------------------------------------------------------
.......

```
kubectl create namespace cert-manager
```

```
helm repo add jetstack https://charts.jetstack.io
```

```
helm repo update
```

```
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.14/deploy/manifests/00-crds.yaml
```

```
helm install cert-manager \
  --namespace cert-manager \
  --version v0.14.0 \
  jetstack/cert-manager
```

```
kubectl get pods --namespace cert-manager
```
###
code cluster-issuer.yaml
------------------------
```
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: <your email> # IMPORTANT: Replace with a valid email from your organization
    privateKeySecretRef:
      name: letsencrypt
    solvers:
    - http01:
        ingress:
          class: nginx
```

```
kubectl apply \
  --namespace ratingsapp \
  -f cluster-issuer.yaml
```

###
code ratings-web-ingress.yaml
-----------------------------
```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ratings-web-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls: 
    - hosts: 
      # - frontend.<ingress ip>.nip.io  # IMPORTANT: update <ingress ip> with the dashed public IP of your ingress, for example frontend.13.68.177.68.nip.ip
      - frontend.52.151.204.185.nip.io  # IMPORTANT: update <ingress ip> with the dashed public IP of your ingress, for example frontend.13.68.177.68.nip.ip
      secretName: ratings-web-cert
  rules:
  # - host: frontend.<ingress ip>.nip.io  # IMPORTANT: update <ingress ip> with the dashed public IP of your ingress, for example frontend.13.68.177.68.nip.ip
  - host: frontend.52.151.204.185.nip.io  # IMPORTANT: update <ingress ip> with the dashed public IP of your ingress, for example frontend.13.68.177.68.nip.ip
    http:
      paths:
      - backend:
          serviceName: ratings-web
          servicePort: 80
        path: /
```

```
kubectl apply \
  --namespace ratingsapp \
  -f ratings-web-ingress.yaml
```

```
kubectl describe cert ratings-web-cert --namespace ratingsapp
```

Step 8 - How to Enable AKS Monitoring
--------------------------------------
```
WORKSPACE=aksworkshop-workspace-$RANDOM
```

```
az resource create --resource-type Microsoft.OperationalInsights/workspaces \
  --name $WORKSPACE \
  --resource-group aksworkshop \
  --location eastus \
  --properties '{}' -o table
```

```
WORKSPACE_ID=$(az resource show --resource-type Microsoft.OperationalInsights/workspaces \
  --resource-group aksworkshop \
  --name $WORKSPACE \
  --query "id" -o tsv)
```

```
az aks enable-addons \
  --resource-group aksworkshop \
  --name aksworkshop-13664 \
  --addons monitoring \
  --workspace-resource-id $WORKSPACE_ID
```




RBAC
----
If you want to configure the Kubernetes RBAC to enable live log. So in addition to the high level overview of the clusters health, we can also view the live log data of the specific containers. To enable and ser permissions for the agent to collect the data, we have to first create a Role that has access to pod logs and events. Then we will assign permissions to user by using Role Binding.

So what exactly is a Kubernetes Role, the RBAC role and the ClusterRole objects will allow you to set up rules that represent a set of permissions.

The main difference between a Role and ClusterRole is that a Role is used with resources in a specific namespace and a ClusterRole is used with non-namespace resources in a cluster.

So what is a Kubernetes Role Binding?
We use a Role Binding to grant the permissions defined in a Role to a user or set of users. A Role Binding contains the list of users, groups or service accounts to whom the role is being granted. Like the Role and ClusterRole, RoleBinding grants permission within a specific namespace and the ClusterRoleBinding grant access to the cluster. So RoleBinding will grant permission within a specific namespace and the ClusterRoleBinding will grant permission to the cluster.

We will use a ClusterRoleBinding bind to bind the clusterRole to all the namespaces in the cluster.

Also in this part will setup clusterRoles and ClusterRoleBindings that are not limited to a specific namespace. We configure ClusterRoles to define permission on namespace. Resources given within individual namespace or across all namespaces. ClusterRoles are used to describe permissions on Cluster scope resources and we then use the ClusterRoleBinding to grant permissions across a whole cluster.


The RoleBinding will grant permission within a specific namespace and a ClusterRoleBinding would give you the permission on the whole cluster.

So let me create a file called logreader-rbac.yaml by using the integrated script editor.

###
code logreader-rbac.yaml
------------------------
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: containerHealth-log-reader
rules:
  - apiGroups: ["", "metrics.k8s.io", "extensions", "apps"]
    resources:
    - "pods/log"
    - "events"
    - "nodes"
    - "pods"
    - "deployments"
    - "replicasets"
    verbs: ["get", "list"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: containerHealth-read-logs-global
roleRef:
  kind: ClusterRole
  name: containerHealth-log-reader
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: User
  name: clusterUser
  apiGroup: rbac.authorization.k8s.io
  
```

