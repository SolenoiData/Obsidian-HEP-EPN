A [volume](https://kubernetes.io/docs/concepts/storage/volumes/) is another important Kubernetes component. Volumes are used for persisting data, for example that generated by applications. Volumes attach physical storage to a pod. This physical storage can be located on a local machine within the Kubernetes cluster, or on a remote machine outside of the Kubernetes cluster, for example on-premise storage or cloud storage. In this manner, *if a database pod gets restarted*, for example, *no data would be* ***lost***.

Kubernetes does not intrinsically manage data persistence, and therefore system administrators are responsible for managing it.

![[Pasted image 20230130102518.png]]