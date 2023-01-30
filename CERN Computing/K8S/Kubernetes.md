# Introduction to Kubernetes

Kubernetes is an open-source container orchestration tool, originally developed by Google, which manages containers, be they Docker containers or some other technology. In short, Kubernetes automates the management, deployment and scaling of applications which can be made up of many containers, and which can run in different environments, such as bare-metal machines, virtual machines, cloud, or hybrid environments.

There is a recent trend to shift away from monolithic application architectures in favour of a microservices approach, increasingly resulting in applications made up of many containers. Relevant examples of applications that can be containerised and orchestrated are BIND DNS services, web-based interfaces with varying load, videoconferencing systems, platforms for online exams, etc. However, the script-based management of such applications, which span across different environments and are composed of many containers, ends up being too complex. Using container orchestration technologies facilitates the management of such applications. Some of the advantages that can be gained from using container orchestration tools are:

- **High availability (or no downtime):** applications are always accessible by users.
- **Scalability:** applications can be quickly scaled up when their load is high, for example because greater numbers of users are trying to access them, and they can also be quickly scaled down when their load goes down.
- **Disaster recovery:** if data is lost, for example because some servers are down, the infrastructure should have a mechanism to back up the data and restore it, so that the applications don’t lose any data and can run from the latest state after recovery.

## Kubernetes Architecture

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

## Kubernetes Virtual Network

Another major component of the Kubernetes cluster is the **virtual network**, which spans all nodes across the cluster and provides connectivity among them. Kubernetes clusters use a **private IP address space** for internal services. In addition, public IP addresses can also be allocated to external services that can be accessed from outside the Kubernetes cluster.

![[Pasted image 20230130095826.png]]

In this sense, Kubernetes assigns each pod a unique IP address. Pods can be accessed internally and externally to the Kubernetes cluster through a **service**. Kubernetes services can provide a public IP address, or an internal IP address within the cluster. For each service, Kubernetes’ **kube-proxy** installs iptables rules, which are used for redirecting the traffic towards the pods; traffic can come from outside the cluster, or from within the cluster.

# Kubernetes Components

![[K8S Components]]

# Configuring Clusters and Creating Resources

## Kubernetes Configuration

In Kubernetes, all configuration requests are sent to the **API server**, which is a control plane process running in the master node. The API server is the main entry point in the cluster. Configuration files must be in either YAML or JSON format, and they can be sent to the API server through either a user interface, a command-line interface or an application programming interface. In our hands-on lab session, we will be using Kubernetes’ command-line interface.

We suggest that you check out our learning units on [YAML](https://e-academy.geant.org/moodle/course/view.php?id=129) and [JSON](https://e-academy.geant.org/moodle/course/view.php?id=66) if you’d like to learn more about these popular formats.

![[Pasted image 20230130111932.png]]

## Kubernetes Configuration File Structure

Kubernetes configuration files have four main parts. The first part is used for declaring the **type** – or “kind” – of component, alongside the **apiVersion** associated to it. The second part is the **metadata**. It includes the name of the component, and sometimes one or more labels that can be defined by the user. The third part is the **specification**, and it includes all configuration attributes. Note that the attributes within the specification part are specific to a given kind of component. For example, a deployment descriptor and a service descriptor will have different attributes. The fourth part of Kubernetes configuration files is the **status**, which is created and added by Kubernetes – we will introduce this too, but let’s start with the first three parts.

![[Pasted image 20230130111957.png]]

The above figure shows an example Kubernetes configuration file in YAML format. With this descriptor, we are sending the Kubernetes API server a request to configure a deployment. In the hands-on lab session, we will use YAML descriptors to deploy some applications in Kubernetes. The YAML syntax is very intuitive, but it is also very strict about indentation, and YAML files with wrong indentation are invalid. In this specific example, we instruct Kubernetes to create three replicas of the pod nginx, based on a container that runs the image nginx version 1.14.2. We also define the required container port configuration.

Configuration requests in Kubernetes are declarative. We specify a set of desired requirements, and Kubernetes tries to meet those requirements. So for example, based on the deployment descriptor in the slide, if we have three nginx pods running and one of them crashes, the controller manager will realise that the desired state is different from the actual state; therefore, it will restart the third pod automatically, in order to achieve the desired state.

The fourth part of Kubernetes configuration files is the status, which is created and added by Kubernetes – so developers do not need to add this part to their Kubernetes descriptors. The status is used by the configuration manager to check the current state against the desired state, and to perform the necessary actions if the actual state does not match the desired state. The status is updated continuously, and this is the basis for Kubernetes’ self-healing capabilities. The status data is gathered from the **etcd**, which is another control plane component. An example of the status is provided in the image below.

![[Pasted image 20230130112014.png]]

The image below shows example descriptor files for a Kubernetes pod and a ConfigMap. As you can see, in the ConfigMap descriptor (on the right) we have basically provided an argument that is needed by the pod; in this example, the argument is player_initial_lives equals three. In the pod descriptor, we mount the ConfigMap as a volume at path etc slash foo. You can find many more examples of Kubernetes objects descriptors on the [Kubernetes online documentation website](https://kubernetes.io/).

![[Pasted image 20230130112035.png]]

## Setting Up Your Own Kubernetes Cluster

Production Kubernetes clusters are normally made up of several nodes running in separate machines (which can be physical or virtual). On occasions, you might need to test something quickly but, as you can imagine, setting up a production Kubernetes cluster is difficult and time-consuming. For these use cases, there are two well-known open-source tools, called **minikube** and **kind**, which allow you to run your very own Kubernetes cluster in your local machine. **minikube** creates a virtual machine where all Kubernetes components run; we end up with a single-node cluster, and all Kubernetes processes run in this single node. **kind**, on the other hand, runs virtualised Kubernetes nodes as Docker containers, and Kubernetes components run as Docker containers in each virtual node. In our hands-on session, we will use minikube to set up our first Kubernetes environment.

## The kubectl Command

You may remember from the last part of the course that all interactions with the Kubernetes cluster pass through the API server process running in the control plane, and that we can communicate with the API server using a command-line interface, a user interface, or an application programming interface. **kubectl** is a command-line interface tool used for interacting with the cluster, and it is the most powerful of all three options. kubectl sends commands to the API server, for example for creating components and deleting components such as pods, deployments and services. kubectl is used in all Kubernetes clusters: production clusters, kind clusters, minikube clusters, etc.

# Resources

The Kubernetes online documentation website provides an interactive environment where you can deploy your own workloads and interact with the kubectl command: [Learn Kubernetes Basics | Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/)

In addition, you may want to check out the online documentation for minikube and kind:  
[Install Tools | Kubernetes](https://kubernetes.io/docs/tasks/tools/)  
[Welcome! | minikube (k8s.io)](https://minikube.sigs.k8s.io/docs/)   
[kind (k8s.io)](https://kind.sigs.k8s.io/)

