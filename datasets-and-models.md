# Datasets and Models

Datasets + Models ALL IN ONE!!! FedML supports comprehensive research-oriented FL datasets and models:

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


We will support more advanced models and datasets, such as FedCV, FedNLP (Transformer), FedGCN, FedGAN. Please stay tuned!