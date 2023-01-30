[Pods](https://kubernetes.io/docs/concepts/workloads/pods/) are the smallest units in Kubernetes, and they basically provide a running environment for containers. In other words, pods introduce an abstraction layer over containers. Usually, there is only one application container running per pod. So, for instance, in the example shown in the figure below, we would place the application logic in one pod, and the database with which this application logic has to interact in a different pod.

![[Pasted image 20230130100755.png]]

In Kubernetes, all interactions are with the Kubernetes layer, so we interact with pods, and not with the containers. Each pod gets its own virtual Kubernetes IP address, so pods can communicate with each other. But pods in Kubernetes are ephemeral; they can die and then are re-created with a new IP address. So we need to guarantee that, even if a pod dies and is re-created with a new IP address, all application pods can still communicate with each other. For this purpose, we use Kubernetes [[Services]].


