1. First we base our docker from a root image.

```Dockerfile
FROM rootproject/root:latest
```

2. We update and install packages

```Dockerfile
RUN apt-get update
RUN apt-get install -y git
RUN apt-get install -y wget tar
```

3. Create anaconda environments
	1. root-analysis-env: We update root and install jupyter notebook. Here we found our first [[Complications]]. We create an environment with the packages specified in [requirements_one.txt](https://github.com/HEP-EPN/ECHEPdocker/blob/main/requirements_one.txt)
	2. python-analysis-env: We create an environment based on the requirements specified in [python-analysis-env.yml](https://github.com/HEP-EPN/ECHEPdocker/blob/main/python-analysis-env.yml)

```Dockerfile
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -b -p /miniconda
ENV PATH=$PATH:/miniconda/condabin:/miniconda/bin

COPY environment_base.yml .
COPY python-analysis-env.yml .
COPY root-analysis-env.yml .
COPY requirements_one.txt .

RUN conda env update -f environment_base.yml

RUN conda config --set channel_priority strict && conda create -c conda-forge --name root-analysis-env root python=3.10 pip
SHELL ["conda","run","-n","root-analysis-env","/bin/bash","-c"]
RUN python -m ipykernel install --name root-analysis-env --display-name "root-analysis-env"
RUN pip install -U -r requirements_one.txt

RUN conda env create -f python-analysis-env.yml
SHELL ["conda","run","-n","python-analysis-env","/bin/bash","-c"]
RUN python -m ipykernel install --name python-analysis-env --display-name "python-analysis-env"

RUN rm Miniconda3-latest-Linux-x86_64.sh
RUN rm environment_base.yml
RUN rm python-analysis-env.yml
RUN rm root-analysis-env.yml
RUN rm requirements_one.txt
```

4. How to implement the environments inside the jupyter notebook.

```Dockerfile
ENV username physicist
ENV NB_UID 1000

RUN useradd -m -s /bin/bash -N -u $NB_UID $username

WORKDIR /home/${username}
USER $username

SHELL ["/bin/bash","-c"]
RUN conda init
RUN echo 'conda activate python-analysis-env' >> ~/.bashrc
```

5. Download the files to process from the web

```Dockerfile
RUN git clone https://github.com/alefisico/machine-learning-das.git /home/${username}/machine-learning-das
```

7. This is where I'm saving the necessary data and locating ourselves inside of a container. This is part of [[Step 3 - Phase 1]]. 

```Dockerfile
RUN wget https://cernbox.cern.ch/remote.php/dav/public-files/MBdDqUTlNiRpuLo/data.tar.gz && tar -xvf data.tar.gz -C /home/${username}/machine-learning-das/ && rm data.tar.gz
```

8. Deploy

```Dockerfile
CMD ["jupyter", "notebook", "--notebook-dir=.", "--ip=0.0.0.0", "--no-browser"]
```
