                                                             # Overview of Kubernetes 
# What is Kubernetes?
Kubernetes (also known as k8s or “kube”) is an open source container orchestration platform that automates many of the manual processes involved in deploying, managing, and scaling containerized applications.<br>

# Features of Kubernetes 

Continues development, integration and deployment<br>

Containerized infrastructure<br> 

Application-centric management <br>

Auto-scalable infrastructure<br> 

Environment consistency across development testing and production <br>

Loosely coupled infrastructure, where each component can act as a separate unit<br>

Higher density of resource utilization <br>

Predictable infrastructure which is going to be created<br> 

# Kubernetes Architecture 

![kubernetes arch](https://user-images.githubusercontent.com/121296386/210240931-42fe304a-9f02-4b3a-9474-0a73090a7d4d.png)
 
**Master Nodes** 

This Master node which control the cluster state and nodes.<br>   

**API Server** - The API Server provides APIs to support lifecycle orchestration scaling,updates and so on for different types of applications. It also acts as the gateway to the cluster, which get the initial request to update the cluster , query to cluster so the API server must be accessible by clients from outside the cluster. Acts as a gatekeeper to authenticate the request to cluster .Clients authenticate <br>via the API Server, and also use it as a proxy/tunnel to nodes and pods (and services).<br>

Whenever pods to be created or new service ,deploy new application ,initially user should interact with master node api server further request will be sent to other processes.It is the entrypoint to the cluster. <br>

**The Scheduler**<br> 

It is a service in master responsible for distributing the workload. It is responsible for tracking utilization of working load on cluster nodes and then placing the workload on which resources are available and accept the workload. In other words, this is the mechanism responsible for allocating pods to available nodes. The scheduler is responsible for workload utilization and allocating pod to new node.<br> 

schedule new pod---> api server---->scheduler ---->where to put the pod.<br> 

**Controller Manager** <br>

This controller manager detects the cluster state changes when the pods die.In general, it can be considered as a daemon which runs in<nonterminating loop and is responsible for collecting and sending information to API server. It works toward getting the shared state of cluster and then make changes to bring the current status of the server to the desired state. The key controllers are replication controller, endpoint controller, namespace controller, and service account controller. The controller manager runs different kind of controllers to handle nodes, endpoints.<br>

Controller manager--> scheduler --> kubelet<br>

**ETCD**<br>

Which is a key value store of cluster state . Cluster changes will be stored in etc in form of key value store. It stores the configuration<br>information which can be used by each of the nodes in the cluster. t is accessible only by Kubernetes API server as it may have some sensitive information. It is a distributed key value Store which is accessible to all.<br> 

**Node process/Worker Node** <br>

Following are the key components of Node server which are necessary to communicate with Kubernetes master.<br>

Each node has multiple pods on it. 3 processes must be installed on every node <br> 

**Container runtime**<br>

Docker which is used as container runtime which helps in running the    encapsulated application containers in a relatively isolated but lightweight operating environment. Container runtime should be installed on every node.<br> 

 **kubelet**<br>  

Kubelet which schedules the pod and interacts with both container and node.Kubelet is responsible for taking the configuration and start the pod with a container inside it. Kubelet is kubernetes process which assign reaources such as ram,cpu from node to container.<br>

**Kube-proxy**<br>

This is a proxy service which runs on each node and helps in making services available to the external host. It helps in forwarding the<br> request to correct containers and is capable of performing primitive load balancing. It makes sure that the networking environment is predictable and accessible and at the same time it is isolated as well. It manages pods on node, volumes, secrets, creating new<br> containers’ health checkup, etc. <br>

**Kubernetes setup in centos 7** 

**Prerequisites** 

--> Multiple Linux servers running CentOS 7 (1 Master Node, Multiple Worker Nodes)<br>

--> A user account on every system with sudo or root privileges <br>

--> The yum package manager, included by default <br>

Command-line/terminal window <br>

**Step 1: Configure Kubernetes Repository** 

Kubernetes packages are not available from official CentOS 7 repositories. This step needs to be performed on the Master Node, and each Worker Node you plan on utilizing for your container setup. Enter the following command to retrieve the Kubernetes repositories.<br> 

These 3 basic packages are required to be able to use Kubernetes.<br> 

Install the following package(s) on each node:<br>

cat &lt;&lt;EOF &gt; /etc/yum.repos.d/kubernetes.repo <br>

[kubernetes] <br>

name=Kubernetes <br>

baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64 <br>

enabled=1 <br>

gpgcheck=1 <br>

repo_gpgcheck=1 <br>

gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg <br>

EOF <br>

**Step 2: Install kubelet, kubeadm and kubectl**<br> 

These 3 basic packages are required to be able to use Kubernetes. Install the following package(s) on each node: <br>

sudo yum install -y kubelet kubeadm kubectl<br> 

systemctl enable kubelet <br>

systemctl start kubelet <br>

**Step 3: Set Hostname on Nodes**<br> 

To give a unique hostname to each of your nodes, use this command:<br> 

sudo hostnamectl set-hostname master-node <br>

Or <br> 

sudo hostnamectl set-hostname worker-node1<br> 

Example:<br> 

sudo vi /etc/hosts <br>

ip master.phoenixnap.com master-node<br> 

ip node1. phoenixnap.com node1 worker-node<br> 

**Step 4: Configure Firewall** <br>
The nodes, containers, and pods need to be able to communicate across the cluster to perform their functions. Firewalld is enabled in CentOS by default on the front-end. Add the following ports by entering the listed commands.<br> 

On the Master Node enter: <br>

sudo firewall-cmd --permanent --add-port=6443/tcp <br>

sudo firewall-cmd --permanent --add-port=2379-2380/tcp<br> 

sudo firewall-cmd --permanent --add-port=10250/tcp <br>

sudo firewall-cmd --permanent --add-port=10251/tcp <br>

sudo firewall-cmd --permanent --add-port=10252/tcp <br>

sudo firewall-cmd --permanent --add-port=10255/tcp <br>

sudo firewall-cmd –reload <br>

Enter the following command on worker node: 

sudo firewall-cmd --permanent --add-port=10251/tcp <br>

sudo firewall-cmd --permanent --add-port=10255/tcp<br> 

firewall-cmd –reload <br>

**Step 5: Update Iptables Settings**<br> 

Set the net.bridge.bridge-nf-call-iptables to '1' in your sysctl config file. This ensures that packets are properly processed by IP tables <br>during filtering and port forwarding. <br>

cat &lt;&lt;EOF &gt; /etc/sysctl.d/k8s.conf <br>

net.bridge.bridge-nf-call-ip6tables = 1 <br>

net.bridge.bridge-nf-call-iptables = 1 <br>

EOF <br>

sysctl –system<br> 

**Step 6: Disable SELinux** <br>

The containers need to access the host filesystem. SELinux needs to be set to permissive mode, which effectively disables its security functions. <br>

sudo setenforce 0<br> 

sudo sed -i ‘s/^SELINUX=enforcing$/SELINUX=permissive/’ /etc/selinux/config<br> 

**Step 7: Disable SWAP**<br> 

The containers need to access the host filesystem. SELinux needs to be set to permissive mode, which effectively disables its security functions. <br>

sudo sed -i '/swap/d' /etc/fstab sudo swapoff –a <br>

**How to deploy kubernetes cluster** <br>

**Step 1: Create Cluster with kubeadm**

sudo kubeadm init --pod-network-cidr=10.244.0.0/16<br>

Initialize a cluster by executing the command <br>

The process might take several minutes to complete based on network speed. Once this command finishes, it displays a kubeadm join message.Make a note of the entry and use it to join worker nodes to the cluster at a later stage. <br>

**Step 2: Manage Cluster as Regular User** <br>

To start using the cluster you need to run it as a regular user by typing:<br>

mkdir -p $HOME/.kube <br>

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config<br> 

sudo chown $(id -u):$(id -g) $HOME/.kube/config <br>

**Step 3: Set Up Pod Network** <br>

A Pod Network allows nodes within the cluster to communicate. There are several available Kubernetes networking options. Use the following<br> command to install the flannel pod network add-on: <br>

sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml<br> 

If you decide to use flannel, edit your firewall rules to allow traffic for the flannel default port 8285. 

**Step 4: Check Status of Cluster** <br>

Check the status of the nodes by entering the following command on the master server:<br> 

sudo kubectl get nodes <br>

Once a pod network has been installed, you can confirm that it is working by checking that the CoreDNS pod is running by typing:<br> 

sudo kubectl get pods --all-namespaces <br>

**Step 5: Join Worker Node to Cluster** <br>

As indicated in Step 1, you can use the kubeadm join command on each worker node to connect it to the cluster.<br> 

kubeadm join --discovery-token cfgrty.1234567890jyrfgd  --discovery-token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443 <br>

**Replace the codes with the ones from your master server. Repeat this action for each worker node on your cluster.** 

 # Main k8s components<br> 

**Node** - Nodes are the physical servers or VMs<br> 

**Pod** – smallest unit of  k8s, abstraction over container.Usually one application per pod.Each pod get its own ip address(internal ip address).whenever the pod
dies new pod with new ip gets created<br>  

**Service** – Assigns a permannent ip address to a pod. Lifecycle of pod and application is not connected. Whenever you want to access the app through browser so<<br> external service can be used and if you wantto access the database with public access then we can create internalservice.<br>
If the url is https://domain-name:port then the request is first forwarded to ingress and then to service.<br>

**ConfigMap** 

If the application need connection to database endpoint, if the database url changes then it would be difficult task to rebuild the image again.<br>
So for this purpose configMap component is used in which we configure database url, db username, db password so that any changes can be applied without the need to<br> rebuild the image.It provides external configuratin for your application.<br> 

**secret** 

Which used to store secret data such as username and password I.e in base64 encoded format.<br> 

Note: If pods deleted automatically associated resources also deleted.<br>
If the node fails automatically the pod will be deleted. <br>

Once the resources being terminated there is no chance to restart them same resources a gain, we can start like same specs other resources from our scripts and<br> commands.The volumes are mapped with pods are available until pod available , if pod or node dies those will be deleted.<br>

**VOLUMES** 

Kubernetes cluster does not take care of data storage . Whenever the pod is restarted the older data is lost .Attaches physical hardware on a <br>harddrive to your pod that could be on local machine I.e same server node where the pod is running or remotei.e outside the kubernetes<br> cluster.<br>

**Types of volumes** 

**emptyDir** − It is a type of volume that is created when a Pod is first assigned to a Node. It remains active as long as the Pod is running on<br> that node. The volume is initially empty and the containers in the pod can read and write the files in the emptyDir volume. Once the<br> Pod is removed from the node, the data in the emptyDir is erased. <br>

**hostPath** − This type of volume mounts a file or directory from the host node’s filesystem into your pod.<br> 

**gcePersistentDisk** − This type of volume mounts a Google Compute Engine (GCE) Persistent Disk into your Pod. The data in a <br>gcePersistentDisk remains intact when the Pod is removed from the node. <br>

**awsElasticBlockStore** − This type of volume mounts an Amazon Web Services (AWS) Elastic Block Store into your Pod. Just like gcePersistentDisk,<br> the data in an awsElasticBlockStore remains intact when the Pod is removed from the node.<br> 

**nfs** − An nfs volume allows an existing NFS (Network File System) to be mounted into your pod. The data in an nfs volume is not erased whengit pull --rebase origin master the<br> Pod is removed from the node. The volume is only unmounted.<br> 

**iscsi** − An iscsi volume allows an existing iSCSI (SCSI over IP) volume to be mounted into your pod. <br>

**flocker** − It is an open-source clustered container data volume manager. It is used for managing data volumes. A flocker volume allows a<br> Flocker dataset to be mounted into a pod. If the dataset does not exist in Flocker, then you first need to create it by using the <br>Flocker API. <br>

**glusterfs** − Glusterfs is an open-source networked filesystem. A glusterfs volume allows a glusterfs volume to be mounted into your pod.<br> 

**rbd** − RBD stands for Rados Block Device. An rbd volume allows a Rados Block Device volume to be mounted into your pod. Data remains preserved<br>  after the Pod is removed from the node. <br> 

cephfs − A cephfs volume allows an existing CephFS volume to be mounted into your pod. Data remains intact after the Pod is removed from the<br> node.<br>  

**gitRepo** − A gitRepo volume mounts an empty directory and clones a git repository into it for your pod to use.<br>  

**secret** − A secret volume is used to pass sensitive information, such as passwords, to pods.<br>  

**persistentVolumeClaim** − A persistentVolumeClaim volume is used to mount a PersistentVolume into a pod. PersistentVolumes are a way for<br> users to “claim” durable storage (such as a GCE PersistentDisk or an iSCSI volume) without knowing the details of the particular cloud<br>  environment.<br>  

**downwardAPI** − A downwardAPI volume is used to make downward API data available to applications. It mounts a directory and writes the<br>  requested<br> data in plain text files.<br>  

**azureDiskVolume** − An AzureDiskVolume is used to mount a Microsoft Azure Data Disk into a Pod.<br>  

**Persistent Volume (PV)** − It’s a piece of network storage that has been provisioned by the administrator. It’s a resource in the cluster<br>  which is independent of any individual pod that uses the PV. <br> 

**Persistent Volume Claim (PVC)** − The storage requested by Kubernetes for its pods is known as PVC. The user does not need to know the<br>  underlying provisioning. The claims must be created in the same namespace where the pod is created.<br>  

kind: PersistentVolume ---------> 1 <br> 
apiVersion: v1 <br> 
metadata: <br> 
   name: pv0001 ------------------> 2 <br> 
   labels: <br> 
      type: local<br>  
spec:<br>  
   capacity: -----------------------> 3 <br> 
      storage: 10Gi ----------------------> 4<br>  
   accessModes: <br>
      - ReadWriteOnce -------------------> 5<br> 
      hostPath: <br>
         path: "/tmp/data01" --------------------------> 6 <br>

 

kind: PersistentVolume → We have defined the kind as PersistentVolume which tells kubernetes that the yaml file being used is to create the Persistent Volume.<br> 

name: pv0001 → Name of PersistentVolume that we are creating.<br> 

capacity: → This spec will define the capacity of PV that we are trying to create.<br> 

storage: 10Gi → This tells the underlying infrastructure that we are trying to claim 10Gi space on the defined path.<br> 

ReadWriteOnce → This tells the access rights of the volume that we are creating.<br> 

path: "/tmp/data01" → This definition tells the machine that we are trying to create volume under this path on the underlying infrastructure.<br> 

Creating PV <br>

kubectl create –f local-01.yaml <br>
persistentvolume "pv0001" created <br>

**Persistent Volume Claim (PVC)**− The storage requested by Kubernetes for its pods is known as PVC. The user does not need to know the<br> underlying provisioning. The claims must be created in the same namespace where the pod is created. <br>

 

kind: PersistentVolume ---------> 1 <br>
apiVersion: v1 <br>
metadata:<br> 
   name: pv0001 ------------------> 2 <br>
   labels: <br>
      type: local <br>
spec: <br>
   capacity: -----------------------> 3 <br>
      storage: 10Gi ----------------------> 4 <br>
   accessModes: <br>
      - ReadWriteOnce -------------------> 5<br> 
      hostPath: <br>
         path: "/tmp/data01" --------------------------> 6<br> 

kind: PersistentVolume → We have defined the kind as PersistentVolume which tells kubernetes that the yaml file being used is to create the <br>Persistent Volume. <br>
name: pv0001 → Name of PersistentVolume that we are creating. <br>

capacity: → This spec will define the capacity of PV that we are trying to create. <br>

storage: 10Gi → This tells the underlying infrastructure that we are trying to claim 10Gi space on the defined path.<br> 

ReadWriteOnce → This tells the access rights of the volume that we are creating.<br> 

path: "/tmp/data01" → This definition tells the machine that we are trying to create volume under this path on the underlying infrastructure.<br> 

volumeMounts: → This is the path in the container on which the mounting will take place.<br> 

Volume: → This definition defines the volume definition that we are going to claim. <br>

persistentVolumeClaim: → Under this, we define the volume name which we are going to use in the defined pod.<br> 

7. **Deployment** 

In order to create the replicas of pod we use deployment component which provide blue print of a pod.You create deployment where you can scale<br> up or down the no of pods required. <br>
Db can’t be replicated via deployment .so this replica set can be done by statefulset for databases such as mysql, elasticsearch, mongo db.<br> 

8. **Statefulset**  

 This take care of databases replucation such as mongodb,elasticsearch, mysql etc this approach of deploying database using statefulset is<br> tedious so it is always recommended to use databases outside kubernetes cluster.<br> 

**Services:** 

To access the application from browser,we need to create a NodePort Service.<br> 

Expose pod with a service (NodePort Service) to access the application externally (from internet)<br> 

port: Port on which node port service listens in Kubernetes cluster internally <br>

targetPort: We define container port here on which our application is running.<br> 

NodePort: Worker Node port on which we can access our application.<br> 

# Create a Pod 

kubectl run <desired-pod-name> --image <Container-Image> --generator=run-pod/v1<br>  

kubectl run my-first-pod –image stacksimplify/kubenginx:1.0.0 --generator=run-pod/v1 <br>

# Expose Pod as a Service 

kubectl expose pod <Pod-Name> --type=NodePort --port=80 --name=<Service-Name><br>  

kubectl expose pod my-first-pod --type=NodePort --port=80 --name=my-first-service<br> 

# Get Service Info  

kubectl get service <br>

kubectl get svc<br>  
 
# Get Public IP of Worker Nodes  

kubectl get nodes -o wide<br> 

kubectl logs <pod-name> kubectl logs my-first-pod <br>

# Connect to a container inside a pod 

kubectl exec -it <pod-name> -- /bin/bash <br> 

kubectl exec -it mypod -- /bin/bash <br>

# Running individual commands in a Container 

kubectl exec -it my-first-pod env <br> 

kubectl exec -it my-first-pod ls <br>

# Get YAML Output of Pod & Service 

# Get pod definition YAML output 

kubectl get pod my-first-pod -o yaml<br> 

# Get service definition YAML output  

kubectl get service my-first-service -o yaml<br> 

# Get all Objects in default namespace  

kubectl get all <br>

# Delete Services  

kubectl delete svc my-first-service<br> 

kubectl delete svc my-first-service2  <br>

kubectl delete svc my-first-service3 <br> 

# Delete Pod  

kubectl delete pod my-first-pod  <br>

# Get all Objects in default namespace 

kubectl get all <br>

# Get list of ReplicaSets 

kubectl get replicaset<br>  

kubectl get rs <br>

# Desc replica set <br>

kubectl describe rs/<replicaset-name> <br>

# Expose ReplicaSet as a Service 

kubectl expose rs <ReplicaSet-Name> --type=NodePort --port=80 --target-port=8080 --name=<Service-Name-To-Be-Created><br> 

kubectl expose rs my-helloworld-rs --type=NodePort --port=80 --target-port=8080 --name=my-helloworld-rs-service<br>  

# Get Service Info  

kubectl get service kubectl get svc  <br>

# Get Public IP of Worker Nodes  

kubectl get nodes -o wide <br>

# ReplicaSet Scalability feature 

Test how scalability is going to seamless & quick 

Update the replicas field in replicaset-demo.yml from 3 to 6. 

Before change spec: replicas: 3 # After change spec: replicas: 6 

Update the ReplicaSet 

# Apply latest changes to ReplicaSet  

kubectl replace -f replicaset-demo.yml<br>  

# Verify if new pods got created 

 kubectl get pods -o wide <br>

# Delete ReplicaSet  

kubectl delete rs <ReplicaSet-Name> <br>

# Delete Service created for ReplicaSet 

# Delete Service 

 kubectl delete svc <service-name>  

# Sample Commands  

kubectl delete svc my-helloworld-rs-service  

[or] kubectl delete svc/my-helloworld-rs-service  

# Verify if Service got deleted  

kubectl get svc
 
# Minikube setup 

To Test on local machine minikube can be installed. Kubectl which will interact with kubernetes cluster . Minikube which has master process<br> and worker proces running in same node <br>

Kubectl get nodes- displays the nodes <br>

Kubectl get services- displays all the services .<br> 

Pod is the samleest unit ,but we are creating deployment abstraction over pods.<br> 

 # Pod creation using Yaml

![tomcat yml](https://user-images.githubusercontent.com/121296386/210214153-5444ae17-2db4-4fb6-8506-4d1a5c238c0d.png)

 kubectl create –f tomcat.yml

**Yaml configuration files** <br> 

 There are 3 parts in these configuration<br> 

 **[root@localhost centos]# cat mongo-configmap.yaml**
 
![mongo-configmap yaml ](https://user-images.githubusercontent.com/121296386/210205592-80516dd9-a620-434c-8428-7977f5c55e9d.png)
kubectl apply -f mongo-confipmap.yaml

**[root@localhost centos]# cat mongodb-deployment.yaml**
 
![mongodb-deployment yaml](https://user-images.githubusercontent.com/121296386/210205601-81403eb5-2fd9-468c-a2d3-bbc217bb3dbe.png)
![mongodb-deployment1 yaml](https://user-images.githubusercontent.com/121296386/210205606-9700fe8b-7b29-4337-90c9-9b985af091c8.png)
 
kubectl apply -f mongodb-deployment.yaml
 
**[root@localhost centos]# cat mongo-express.yaml** 

![mongo-express yaml](https://user-images.githubusercontent.com/121296386/210211989-4d43c911-021d-4df2-921f-5aac88e39602.png)

![mongo-express yaml1](https://user-images.githubusercontent.com/121296386/210212046-adcf06b7-c064-4e00-bcaf-5ed58768f903.png)

kubectl apply -f mongodb-deployment.yaml

**[root@localhost centos]# cat mongo-secret.yaml** 

![mong-secret yaml](https://user-images.githubusercontent.com/121296386/210212066-24ebec2f-0f9a-4c94-bfb1-7747a15d9a56.png)

kubectl apply -f mongo-secret.yaml

**[root@localhost centos]# cat nginx-deployment.yaml** 

![nginx-deployment yaml](https://user-images.githubusercontent.com/121296386/210212396-94b2e0c2-fe8a-4171-b834-2da83f60268d.png)
 
kubectl apply -f nginx-deployment.yaml<br>

**Labels and Selectors:**

Labels are key-value pairs which are attached to pods, replication controller and services. They are used as identifying attributes for objects such as pods and<br> replication controller. They can be added to an object at creation time and can be added or modified at the run time.<br>
 
**Selectors:**<br>

Labels do not provide uniqueness. In general, we can say many objects can carry the same labels. Labels selector are core grouping primitive in Kubernetes.They are<br> used by the users to select a set of objects.<br>

Kubernetes API currently supports two type of selectors −<br>

**Equality-based selectors<br>

Equality-based label selectors work by specifying an exact value that you want to match against. If you provide multiple selectors, all of them must be satisfied to<br>qualify as a match.<br>

**Set-based selectors**<br>

In set based selectors you can specify multiple values in a single selector, and only one of them needs to match for the object to<br> qualify.

NOTE: Deployment manages pods,Replicatset manages pod,pod is an abstraction of container,Demo Project:<br> 

Mongo express application from browser ---> Mongo express external service--->Mongo pod-->Mongo db internal service--->Mongodb pod<br>
 
# What is Namespace? 

Namespaces are a way to organize clusters into virtual sub-clusters — they can be helpful when different teams or projects share a Kubernetes cluster. Any number of<br> namespaces are supported within a cluster, each logically separated from others but with the ability to communicate with each other.<br>

**Kubernetes-dashboard** – This namspace is specific to mini-kube.<br> 

**Kube-system** – Do not create or modify in kube system, kubectl , manages master process.<br> 

**Kube-public** – It contains publicly accessible data, configmap which contains clusterinfo.<br> 

**Kube-node-lease** –heartbeats of node,each node has associated lease object in namespace,determines the availability of a node.<br>

**Default-namespace** – resources you created are located here.<br> 

Kubectl create namespace <nameof namespace><br>

**Why should we use namespace.**<br> 

To group the resources in namespace.In order to avoid all the resources in same place.<br> 

In order to avoid the conflicts between the teams and application. In order to differentiate the different application.<br> 

Resource sharing and development.<br>

Blue/green deployment<br> 

Access and resource limits on namespace.<br>

Each namespace must define own configMap.<br>

You can’t access most resources from another namespace.<br> 

**Access service in another namespace.** 

Command to get all the namespace<br>

Kubectl get all –n my-namespace.(To get all the namespace )<br> 

Kubectl apply –f mysql-config.yaml --namespace=my-namespace.<br>

Kubectl get configmap –n my-namespace.<br> 

We can also specify namespace in configuration file other than command line.<br> 

To change the active namespace we must install kubectx tool – Kubens namespace<br> 

**Ingress vs External service** 

Whenever we want to access the application via domain name instead of port then we can use ingress and make external service as internal service.
 
**Ingress**

You can also use Ingress to expose your Service. Ingress is not a Service type, but it acts as the entry point for your cluster. It lets you consolidate your routing rules into a single resource as it can expose multiple services under the same IP address. 
Usually the request from browser first reaches to ingress component and then to external service and then to pod in case of http://<domain-name>:ip 

# Ingress and internal service configuration** 
 
![Ingress and internal service ](https://user-images.githubusercontent.com/121296386/210246845-4cf66cfe-55ed-4362-8b6c-722536326ee6.png)

# Ingress Controller 

Evaluates all the rules,manages redirections,entrypoint to cluster.

![Ingress controller](https://user-images.githubusercontent.com/121296386/210247208-452742e4-cc5e-48fa-afe7-91750f387ad6.png)
 
![Ingress controller 1](https://user-images.githubusercontent.com/121296386/210246974-aa504292-4217-4d57-b33a-71729ad68539.png)

# Ingress yaml file configuration** 

Kubectl get all –n kubernetes-dashboard 
 
![Ingress yaml config](https://user-images.githubusercontent.com/121296386/210247547-7a47a5ac-8aaf-4406-9c98-a27afb635d11.png)

Kubectl apply –f dashboard-ingress.yaml 

Kubectl get ingress –n kubernetes-dashboard –watch 

We need to mention the ip in /etc/hosts  

Ingress also has backend 

Kubectl describe ingress dashboard-ingress –n kubernetes-dashboard 

# Helm package manager 

Helm which is a package manager of kubernetes and helm charts which is a bundle of yaml files,create your own helm charts with helm and push them to repository download and use existing ones. 

Helm charts can be shared to public or private registeries and which can be used later. 


# K8s services 

Kubernetes services connect a set of pods to an abstracted service name and IP address. Services provide discovery and routing between pods. For example, services connect an application front-end to its backend, each of which running in separate deployments in a cluster. Services use labels and selectors to match pods with other applications. The core attributes of a Kubernetes service are: 

A label selector that locates pods 

The clusterIP IP address and assigned port number 

Port definitions 

Optional mapping of incoming ports to a targetPort 

Services can be defined without pod selectors. For example, to point a service to another service in a different namespace or cluster. 

ClusterIP. Exposes a service which is only accessible from within the cluster. 

NodePort. Exposes a service via a static port on each node’s IP. 

LoadBalancer. Exposes the service via the cloud provider’s load balancer. 

ExternalName. Maps a service to a predefined externalName field by returning a value for the CNAME record. 

 

 

Kubernetes — Service Types 

1. ClusterIP 

ClusterIP is the default and most common service type. 

Kubernetes will assign a cluster-internal IP address to ClusterIP service. This makes the service only reachable within the cluster. 

You cannot make requests to service (pods) from outside the cluster. 

You can optionally set cluster IP in the service definition file. 

Use Cases 

Inter service communication within the cluster. For example, communication between the front-end and back-end components of your app. 

Example 

![clusterip](https://user-images.githubusercontent.com/121296386/210247919-20de7f3d-8bea-4eba-b504-ce01319ec112.png)

2. NodePort 

NodePort service is an extension of ClusterIP service. A ClusterIP Service, to which the NodePort Service routes, is automatically created. 

It exposes the service outside of the cluster by adding a cluster-wide port on top of ClusterIP. 

NodePort exposes the service on each Node’s IP at a static port (the NodePort). Each node proxies that port into your Service. So, external traffic has access to fixed port on each Node. It means any request to your cluster on that port gets forwarded to the service. 

You can contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>. 

Node port must be in the range of 30000–32767. Manually allocating a port to the service is optional. If it is undefined, Kubernetes will automatically assign one. 

If you are going to choose node port explicitly, ensure that the port was not already used by another service. 

Use Cases 

When you want to enable external connectivity to your service. 

Using a NodePort gives you the freedom to set up your own load balancing solution, to configure environments that are not fully supported by Kubernetes, or even to expose one or more nodes’ IPs directly. 

Prefer to place a load balancer above your nodes to avoid node failure. 

Example 

![Nodeport](https://user-images.githubusercontent.com/121296386/210247958-15e0207f-5245-45bf-8005-d9ef4dc185df.png)
 
3. LoadBalancer 

LoadBalancer service is an extension of NodePort service. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created. 

It integrates NodePort with cloud-based load balancers. 

It exposes the Service externally using a cloud provider’s load balancer. 

Each cloud provider (AWS, Azure, GCP, etc) has its own native load balancer implementation. The cloud provider will create a load balancer, which then automatically routes requests to your Kubernetes Service. 

Traffic from the external load balancer is directed at the backend Pods. The cloud provider decides how it is load balanced. 

The actual creation of the load balancer happens asynchronously. 

Every time you want to expose a service to the outside world, you have to create a new LoadBalancer and get an IP address. 

Use Cases 

When you are using a cloud provider to host your Kubernetes cluster. 

This type of service is typically heavily dependent on the cloud provider. 

Example 

![loadbalancer](https://user-images.githubusercontent.com/121296386/210248003-4c13ac1a-6cef-4a72-ac48-bd74814d55e1.png)
 
4. ExternalName 

Services of type ExternalName map a Service to a DNS name, not to a typical selector such as my-service. 

You specify these Services with the `spec.externalName` parameter. 

It maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. 

No proxying of any kind is established. 

Use Cases 

This is commonly used to create a service within Kubernetes to represent an external datastore like a database that runs externally to Kubernetes. 

You can use that ExternalName service (as a local service) when Pods from one namespace to talk to a service in another namespace. 

Example 

![externalip](https://user-images.githubusercontent.com/121296386/210247945-29b7abf8-a5d0-404c-b3a2-4ae26e15d45f.png)
 

 

 

 

 

 

 

 

 
