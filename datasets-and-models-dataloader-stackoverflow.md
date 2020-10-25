# fedml_api.data_preprocessing.stackoverflow_lr.data_loader

``` python
fedml_api.data_preprocessing.stackoverflow_lr.
load_partition_data_federated_shakespeare(
    dataset, 
    data_dir, 
    client_number=None, 
    batch_size=DEFAULT_BATCH_SIZE)
```

<p>Load stackoverflow dataset for tag prediction task (logistic regression) in both global and local format.</p>

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
| output_dim | Output dimension of tag prediction. It is the length of tag vocabulary plus one (for out of vocabulary tag). |


------------------------------------------------------------


# fedml_api.data_preprocessing.stackoverflow_nwp.data_loader

``` python
fedml_api.data_preprocessing.stackoverflow_nwp.
load_partition_data_federated_shakespeare(
    dataset, 
    data_dir, 
    client_number=None, 
    batch_size=DEFAULT_BATCH_SIZE)
```

<p>Load stackoverflow dataset for next word prediction task in both global and local format.</p>

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
| vocab_len | The length of word vocabulary plus one (for out of vocabulary word).|