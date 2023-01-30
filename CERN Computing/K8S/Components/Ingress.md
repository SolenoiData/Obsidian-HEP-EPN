[Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) redirects a URL call to the IP address of an external service; this process is transparent to users, who only need to know the service’s URL.

External services are accessible through the node’s IP address and a specific port. But, on some occasions, we might want the [[Services]] to be accessed through a URL rather than an IP address. This could be the case of a web application, where users should be able to write a URL in their web browser and access the application. In Kubernetes, this is possible thanks to the ingress component. 

![[Pasted image 20230130102019.png]]

