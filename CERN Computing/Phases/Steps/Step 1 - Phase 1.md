Since we are using Argo Workflows we can incorporate our  [[HEPdocker]] image as follows:

```yaml
	- name: runml-template
	  script:
		image: alefisico/echep:v0     <- Specify the image
		command: [bash]
		source: |
			sudo chown $USER /mnt/vol
	
	volumeMounts:
	- name: task-pv-storage
	  mountPath: /mnt/vol
	resources:
		requests:
			memory: 1.7Gi
			cpu: 750m
```


