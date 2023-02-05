[[Phase 1]]:

1. Download the docker image 
2. Run docker on the cluster nodes in Kubernetes with Argo
3. Save the output files inside the container 
4. Save the output files on a persistent volume

[[Phase 2]]:

1. Delete the files inside the docker image
2. Read CMS files from outside the container in order to process them
3. Implement saving the output files outside of the container

[[Phase 3]]:

1. Work with heavier input data [TB]