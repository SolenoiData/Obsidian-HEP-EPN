[Services](https://kubernetes.io/docs/concepts/services-networking/service/) provide a static IP address that can be attached to each pod, and point to specific container ports. Even if the pod dies, the service and its IP address will remain. 

We can broadly differentiate between external and internal services. **External services** can be accessed from outside the Kubernetes cluster. This would be the case of a web application, for example. **Internal services** can only be accessed within the cluster, and they are useful for communication among [[Pods]]. An example could be a database pod, which we do not want to be accessible from outside the cluster, but which needs to be internally accessible to some applications.

![[Pasted image 20230130100950.png]]

