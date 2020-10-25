# fedml_api.data_preprocessing.fed_cifar100.data_loader

``` python
fedml_api.data_preprocessing.fed_cifar100.
load_partition_data_federated_cifar100(
    dataset, 
    data_dir, 
    client_number=DEFAULT_CLIENT_NUMBER, 
    batch_size=DEFAULT_BATCH_SIZE)
```

<p>Load CIFAR100 dataset in both global and local format.</p>

|__Params__ | |
|-|-|
| dataset | Deprecated, will be removed later. |
| data_dir | The path to the dataset. |
| client_number | Deprecated, will be removed later. |
| batch_size | Batch size. |

| __Returns__ | |
|-|-|
| client_number| The number of clients in total. |
| train_data_num | The number of training samples returned. |
| test_data_num | The number of test samples returned. |
| train_data_global | Global train dataset. |
| test_data_global |  Global test dataset. |
| data_local_num_dict | Dict containing the number of samples in each client. |
| train_data_local_dict | Dict containing train dataset of each client. |
| test_data_local_dict | Dict containing test dataset of each client. |
| class_num | The number of classes. |
