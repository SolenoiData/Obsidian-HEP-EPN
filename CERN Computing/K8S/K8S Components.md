The main components of Kubernetes are: 

- [[Pods]]
- [[Services]]
- [[Ingress]]
- [[ConfigMaps]]
- [[Secrets]]
- [[Volumes]]
- [[Deployments]]
- [[StatefulSets]]

## Wrap-up

Kubernetes has many functionalities, and one of them is resilience. We can use deployments and StatefulSets, which we have just learned about, in order to distribute our applications over several nodes. In this way, if one of the nodes is down, we will have replicas of the application running on a different node of the cluster, and there won’t be downtime for users.

![[Pasted image 20230130102919.png]]


