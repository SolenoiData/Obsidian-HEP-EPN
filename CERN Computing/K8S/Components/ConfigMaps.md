[ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) store configuration data, with the aim of decoupling environment-specific configuration from container images, so effectively we are decoupling configuration data and application code. ConfigMaps are consumed by pods, for example as environment variables, command-line arguments, or configuration files.

In our example diagram, if we changed the name of the database [[Services]] without using a ConfigMap, we would have to change the name of the endpoint in our application logic, and then rebuild the image. If we used a ConfigMap instead, we would only have to change the name of the endpoint in the ConfigMap, and the application pod would just consume the new endpoint name from the ConfigMap, without having to implement any changes to the application image.

![[Pasted image 20230130102140.png]]
