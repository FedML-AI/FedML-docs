# fedml_api.data_preprocessing.FederatedEMNIST.data_loader
<!-- ``` python
fedml_api.data_preprocessing.FederatedEMNIST.data_loader.load_partition_data_distributed_federated_emnist(
    process_id, 
    dataset, 
    data_dir, 
    client_number=DEFAULT_CLIENT_NUMBER, 
    batch_size=DEFAULT_BATCH_SIZE)
```
<p>Load EMNIST dataset for each client.</p>

|__Params__ | |
|-|-|
| process_id | The client index to load emnist dataset. When process_id = 0, the function returns the whole emnist dataset. |
| dataset | Deprecated, will be removed later. |
| data_dir | The path to the dataset. |
| client_number | Deprecated, will be removed later. |
| batch_size | Batch size. |

| __Returns__ | |
|-|-|
| client_number| The number of clients in total. |
| train_data_num | The number of training samples returned. |
| train_data_global | The number of training samples in total. |
| test_data_global | The number of test samples in total. |
| local_data_num | Deprecated, will be removed later.  |
| train_data_local | Train dataset returned. |
| test_data_local | Test dataset returned. |
| class_num | The number of classes. | -->


----------------------------------

``` python
fedml_api.data_preprocessing.FederatedEMNIST.data_loaderload_partition_data_federated_emnist(
    dataset, 
    data_dir, 
    client_number=DEFAULT_CLIENT_NUMBER, 
    batch_size=DEFAULT_BATCH_SIZE)
```
<p>Load EMNIST dataset in both global and local format.</p>

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
