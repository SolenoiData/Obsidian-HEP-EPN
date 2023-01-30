Kubernetes is an open-source container orchestration tool, originally developed by Google, which manages containers, be they Docker containers or some other technology. In short, Kubernetes automates the management, deployment and scaling of applications which can be made up of many containers, and which can run in different environments, such as bare-metal machines, virtual machines, cloud, or hybrid environments.

There is a recent trend to shift away from monolithic application architectures in favour of a microservices approach, increasingly resulting in applications made up of many containers. Relevant examples of applications that can be containerised and orchestrated are BIND DNS services, web-based interfaces with varying load, videoconferencing systems, platforms for online exams, etc. However, the script-based management of such applications, which span across different environments and are composed of many containers, ends up being too complex. Using container orchestration technologies facilitates the management of such applications. Some of the advantages that can be gained from using container orchestration tools are:

- **High availability (or no downtime):** applications are always accessible by users.
- **Scalability:** applications can be quickly scaled up when their load is high, for example because greater numbers of users are trying to access them, and they can also be quickly scaled down when their load goes down.
- **Disaster recovery:** if data is lost, for example because some servers are down, the infrastructure should have a mechanism to back up the data and restore it, so that the applications don’t lose any data and can run from the latest state after recovery.

# Kubernetes Architecture

A Kubernetes **cluster** is formed by one (or more) **master node**(s) connected to one or more **worker nodes**. Nodes in Kubernetes can be either physical or virtual machines. Worker nodes are where applications are running. Each worker node has containers of different applications running on it. The number of containers running on each worker node depends on the workload on each of the worker nodes. Each worker node also has a **kubelet** process running on it. The kubelet is a process that enables the communication among Kubernetes nodes. The kubelet also enables the execution of tasks on those nodes, such that applications can run on them.

The master node is more important than individual worker nodes, because it runs processes that are necessary for the correct functioning of the Kubernetes cluster. Also, if we lose access to the cluster master node, then we won’t be able to access the cluster anymore.

![[Pasted image 20230130095700.png]]

Worker nodes need to run higher workloads than the master node, and therefore these are normally bigger and have more resources.

There are several important processes that run on the master node. For example, there is an **API server** which itself runs in a container and is the entry point to the Kubernetes cluster. This is the process that different Kubernetes clients talk to, for example:

-   UIs, if we are using a tool with a user interface, such as a dashboard
-   APIs, if we are using scripts, and
-   Command-line interface tools.

Another containerised process that runs on the master node is the **controller manager**. This process keeps track of whatever is happening in the cluster, for example if a container is down and it needs to be restarted. There is also a **scheduler**, which is responsible for scheduling containers on different nodes, based on the workload that needs to be scheduled and the available resources at the nodes. Another important component is the **etcd** key-value storage, which holds the current state of the Kubernetes cluster, for example the configuration data and the status data of each node and container. The etcd can be used for recovering the cluster state at any time.

![[Pasted image 20230130095735.png]]

# Kubernetes Virtual Network

Another major component of the Kubernetes cluster is the **virtual network**, which spans all nodes across the cluster and provides connectivity among them. Kubernetes clusters use a **private IP address space** for internal services. In addition, public IP addresses can also be allocated to external services that can be accessed from outside the Kubernetes cluster.

![[Pasted image 20230130095826.png]]

In this sense, Kubernetes assigns each pod a unique IP address. Pods can be accessed internally and externally to the Kubernetes cluster through a **service**. Kubernetes services can provide a public IP address, or an internal IP address within the cluster. For each service, Kubernetes’ **kube-proxy** installs iptables rules, which are used for redirecting the traffic towards the pods; traffic can come from outside the cluster, or from within the cluster.

# Kubernetes Components

![[K8S Components]]

