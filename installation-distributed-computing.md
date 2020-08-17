# FedML Distributed Computing Hardware and Software Configuration

## 1. Hardware Requirements
![multi-gpu-server](./image/multi-gpu-topo.png)

The computing architecture is comprised of
1. N compute nodes, each compute node is a multi-GPU server node (e.g., 8 x NVIDIA V100). 
2. A head node (login, testing) 
3. A centralized fault-tolerant file-server (e.g., NFS). In machine learning setting, this is used to share large-scale dataset among compute nodes.

Note: 1) Any FL topology can be ran in this physical configuration (e.g. decentralized FL); 2) High bandwidth configuration such as InfiniBand is optional.

### **- NFS (Network File System) Configuration**
Please google related installment instructions according to the OS version of your server.

### **- change SSH configuration for your cluster**

- On your local computer (MAC/Windows), generate the public key:
> mkdir ~/.ssh
> ls ~/.ssh
> ssh-keygen -t rsa
> vim ~/.ssh/id_rsa.pub

- Login to the server-side:
> ssh chaoyang@xxx.usc.edu

- modify the "authorized_keys"
> vim ~/.ssh/authorized_keys


Paste the string in "id_rsa.pub" file on your local computer to the server side "authorized_keys" file, and save the authorized_keys
> chmod 700 ~/.ssh/
> chmod 600 ~/.ssh/authorized_keys


- login out and login again, you will find you don't need to input the passwords anymore.

For other nodes on your server, use a similar method to configure the SSH.


## 2. Software Configuration

Code implementation is based on PyTorch >= 1.4.0, MPI4Py >= 3.0.3 (https://pypi.org/project/mpi4py), and Python >= 3.7.4.

The experiment tracking platform is supported by Weights and Bias: https://www.wandb.com/

Here is a step-by-step configuration to help you quickly set up a multi-GPU computing environment.
### **- Install Conda**

https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html
https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html

### **- PyTorch**

> conda install pytorch torchvision cudatoolkit=10.2 -c pytorch

### **- MPI4py**
> conda install -c anaconda mpi4py

### **- Weights and Bias**
> pip install --upgrade wandb

### **- config MPI host file**
Modify the hostname list in "mpi_host_file" to correspond to your actual physical network topology (in linux, use "hostname" command to get the hostname).
An example: Let us assume a network has a management node and four compute nodes (hostname: node1, node2, node3, node4).
If you want use node1 and node2 to run our program, the "mpi_host_file" should be:
> node1 \
> node2 \
> node3


### **- install other required packages**
> pip install -r requirements.txt


### **- Experimental Tracking Platform (report real-time result to wandb.com)**
wandb login ee0b5f53d949c84cee7decbe7a629e63fb2f8408

(please modify the ID to your own)

## 3. Dataset Preparation
"FedML/data" directory contains all datasets that we suggest. Please cd to corresponding directory and run the *.sh script. 
The data will be download to the same path as the *.sh file. Here is an example for CIFAR-10:

```
cd data/cifar10
sh download_cifar10.sh
```
After executing the above comands, the CIFAR-10 dataset is stored at "FedML/data/cifar10".

## 4. Run Experiments to Check the Configuration Correctness
To check if the above configuration is correct, we use the FedAvg algorithm as an example.

```
cd fedml_experiments/distributed/fedavg
sh run_fedavg_distributed_pytorch.sh 1 8 resnet56 homo 100 20 64 0.001 cifar10 "./../../../data/cifar10"
```
Note that the "run_fedavg_distributed_pytorch.sh" script should be executed at the path "fedml_experiments/distributed/fedavg".

If you find any issue in above steps, please feel free to post comments. New comments will be transformed as GitHub issues.
