[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) are consumed by [[Pods]], just like ConfigMaps. However, Kubernetes does not encrypt secret components out of the box. It is good practice to encrypt secrets using a third-party tool.

Sometimes, configuration data can include sensitive data such as usernames and passwords. These can also change during the lifecycle of the application. But placing this kind of data in plain-text format in a ConfigMap is insecure and it is not good practice.  For this purpose, Kubernetes has another object called a secret, which is similar to a ConfigMap but is used to store secret data in base-64 encoded format. 

![[Pasted image 20230130102331.png]]
