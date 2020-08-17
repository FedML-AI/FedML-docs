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
After installation, please change the ssh configuration for your cluster so as to login without passwords.

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

<p style='color:red'>Code implementation is based on PyTorch >= 1.4.0, MPI4Py >= 3.0.3, and Python >= 3.7.4.</p>
(We will support RPC in near future.)

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

### **- Install Other Required Packages**

> cd fedml_experiments/distributed \
> pip install -r requirements.txt


(please modify the ID to your own)

## 3. Dataset Preparation
"FedML/data" directory contains all datasets that we suggest. Please cd to corresponding directory and run the *.sh script. 
The data will be downloaded to the same path as the *.sh file. Here is an example for CIFAR-10:

> cd data/cifar10 \
> sh download_cifar10.sh

After executing the above comands, the CIFAR-10 dataset is stored at "FedML/data/cifar10".

## 4. Run Experiments to Check the Configuration Correctness
To check if the above configuration is correct, we use the FedAvg algorithm as an example.
> cd fedml_experiments/distributed/fedavg

### **- Config MPI Host File**
Modify the hostname list in "mpi_host_file" to correspond to your actual physical network topology (in linux, use "hostname" command to get the hostname).
An example: Let us assume a network has a management node and four compute nodes (hostname: node1, node2, node3, node4).
If you want use node1, node2, and node3 to run our program, the "mpi_host_file" should be:
> node1:4 \
> node2:2 \
> node3:2

(Format: hostname:process_number; each line is for a compute node)

### **- Experimental Logging and Tracking Platform**
Please apply ID from [wandb.com](https://www.wandb.com), and then login as follows.

> wandb login ee0b5f53d949c84cee7decbe7a629e63fb2f8408


### **- Run Experiment**
> sh run_fedavg_distributed_pytorch.sh 10 1 8 resnet56 homo 100 20 64 0.001 cifar10 "./../../../data/cifar10"

Note that 
- the "run_fedavg_distributed_pytorch.sh" script should be executed at the path "fedml_experiments/distributed/fedavg".
- please match the *.sh parameters (SERVER_NUM, GPU_NUM_PER_SERVER, WORKER_NUM) to your mpi host file before running. Otherwise, too many workers may run out of the GPU memory. We suggest using a small worker number to test the GPU memory cost first, and then scaling up to your requirement.


If you find any issue in above steps, please feel free to post comments. New comments will be transformed as GitHub issues.
