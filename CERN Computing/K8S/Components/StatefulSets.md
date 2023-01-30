[StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) offer similar functionality to deployments, but make sure that read and write data operations are synchronised among pods.

There are some types of pods that can’t be replicated using [[Deployments]]. For example, database [[Pods]], because they have a state. In the case of databases, their state is their data at any given time. If we had several copies of database pods using the same data storage, we would need some mechanism to manage which replica of the database pod can write or read information, for example, in order to avoid data inconsistencies. Stateful applications such as databases should be created using StatefulSets, **not deployments**. 

![[Pasted image 20230130102733.png]]
