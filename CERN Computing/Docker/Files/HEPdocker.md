1. First we base our docker from a root image.

![[Pasted image 20230129181800.png]]

2. We update and install packages

![[Pasted image 20230129183626.png]]

3. Create anaconda environments
	1. root-analysis-env: We update root and install jupyter notebook. Here we found our first [[Complications]]. We create an environment with the packages specified in [requirements_one.txt](https://github.com/HEP-EPN/ECHEPdocker/blob/main/requirements_one.txt)
	2. python-analysis-env: We create an environment based on the requirements sepcified in [python-analysis-env.yml](https://github.com/HEP-EPN/ECHEPdocker/blob/main/python-analysis-env.yml)

![[Pasted image 20230129181915.png]]

4. How to implement the environments inside the jupyer notebook.

![[Pasted image 20230129183436.png]]

5. Download the files to process from the web

![[Pasted image 20230129184115.png]]

7. This is where I'm saving the necessary data and locating ourselves inside of a container. This is part of [[Step 3 - Phase 1]]. 

![[Pasted image 20230129184126.png]]

8. Deploy

![[Pasted image 20230129184142.png]]
