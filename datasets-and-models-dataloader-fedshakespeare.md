# fedml_api.data_preprocessing.fed_shakespeare.data_loader

``` python
fedml_api.data_preprocessing.fed_shakespeare.
load_partition_data_federated_shakespeare(
    dataset, 
    data_dir, 
    client_number=None, 
    batch_size=DEFAULT_BATCH_SIZE)
```

<p>Load shakespeare dataset in both global and local format.</p>

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
| vocab_len | The number of characters in vocabulary. |
