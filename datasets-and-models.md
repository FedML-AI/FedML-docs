# Datasets and Models

Datasets + Models ALL IN ONE!!! FedML supports comprehensive research-oriented FL datasets and models:

## **Models**

#### Federated Learning (FedAvg, FedOpt, FedNova, etc) or Decentralized Federated Learning:
- cross-device CV: Federated EMNIST + CNN (2 conv layers)
- cross-device CV: CIFAR100 + ResNet18 (Group Normalization)
- cross-device NLP: shakespeare + RNN (bi-LSTM)
- cross-device NLP: stackoverflow (NWP) + RNN (bi-LSTM)
- cross-silo CV: CIFAR10, CIFAR100, CINIC10 + ResNet
- cross-silo CV: CIFAR10, CIFAR100, CINIC10 + MobileNet
- linear: MNIST + Logistic Regression
- linear: Synthetic + Logistic Regression

Please check `create_model(args, model_name, output_dim)` and `load_data(args, dataset_name)` at `fedml_experiments/distributed/fedavg/main_fedavg.py` for details.



#### FedNAS
- cross-silo CV: CIFAR10, CIFAR100, CINIC10 + ResNet
- cross-silo CV: CIFAR10, CIFAR100, CINIC10 + MobileNet

#### Vertical Federated Learning:
- lending_club_loan + VFL
- NUS_WIDE + VFL

#### Split Learning:
- cross-silo CV: CIFAR10, CIFAR100, CINIC10 + ResNet
- cross-silo CV: CIFAR10, CIFAR100, CINIC10 + MobileNet

---------------------------------------------------------


## **Datasets**

FedMl suppports multiple datasets for users to experiment on various federated learning tasks. Currently, our framework covers datasets in the field of computer vision and natural language processing.
<!-- 
## **Datasets list** -->

#### CV

 - **[EMNIST](https://github.com/FedML-AI/FedML/tree/master/data/FederatedEMNIST)**
EMNIST dataset extends MNIST dataset with upper and lower case English characters. Suggested by "Adaptive Federated Optimization" (https://arxiv.org/abs/2003.00295, Google AI).


- **[CIFAR-100](https://github.com/FedML-AI/FedML/tree/master/data/fed_cifar100)**
CIFAR-100 dataset consists of 100 image classes with each containing 600 images. Suggested by "Adaptive Federated Optimization" (https://arxiv.org/abs/2003.00295, Google AI).

#### NLP

- **[Shakespeare](https://github.com/FedML-AI/FedML/tree/master/data/fed_shakespeare)**
Shakespeare dataset is built from the collective works of William Shakespeare. This dataset is used to perform tasks of next character prediction. Suggested by "Adaptive Federated Optimization" (https://arxiv.org/abs/2003.00295, Google AI).


- **[Stack Overflow](https://github.com/FedML-AI/FedML/tree/master/data/stackoverflow)**
Stack Overflow dataset originally hosted by Kaggle consists of questions and answers from the website Stack Overflow. This dataset is used to perform two tasks: tag prediction via logistic regression and next word prediction. Suggested by "Adaptive Federated Optimization" (https://arxiv.org/abs/2003.00295, Google AI).


## **Datasets download**

For datasets suggested by Google AI, in order to support more frameworks like PyTorch, we loaded data from [TensorFlow Federated (TFF)](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets) load_data API to get the unzipped raw data in h5 format saved it on google drive.

Scripts for downloading datasets are provided under the path of [/Fedml/data/](https://github.com/FedML-AI/FedML/tree/master/data) . 

The structure of each folder is like:
```bash
/FedML/data/{dataset}

   ├── dataset.py
   ├── download_{dataset}.sh
   └── README.md

```

Users can grab the dataset in either of three ways:

#####  1.Download through bash script from google drive
```bash
sh download_{dataset}.sh
```
<span style='color:red'>Due to the download limit of google drive, this way may receive error code of 403.</span> Please check the download log and the downloaded file size (supposed to larger than 4 KBs) to ensure the dataset is downloaded safe and sound :). Or you may need to use the methods below to download them.

#####  2.Manually download from google drive
The link to google drive is included in **README.md** in each dataset folder. 
You can also find all the datasets here: https://drive.google.com/drive/folders/1FBaMq4tIdvBcfSAr6EUIAjWLZbtxaP2X?usp=sharing.

#####  3.Run python script to get the file from TFF API
```python
python dataset.py
```
TFF dependency is required to get this script work. Please check it out: https://www.tensorflow.org/federated/install. 

We will support more advanced models and datasets, such as FedCV, FedNLP (Transformer), FedGCN, FedGAN. Please stay tuned!

## **Datasets usage** - dataloader

Datasets are loaded by data_loader.py (located under fedml_api/data_preprocessing/{dataset}/.) All the returned data are in the form of pytorch [Dataloader](https://pytorch.org/docs/stable/data.html).

- [emnist]
- [cifar100]
- [shakspeare]
- [stackoverflow]