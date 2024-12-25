# Pipeline範例
Pipeline 是一種高度靈活且通用的框架，可以根據需求增減步驟。例如，當處理流程較為簡單時，可以減少步驟；而當需求更複雜時，可以添加更多任務。然而，實際應用中，數據處理的 Pipeline 通常會被設計為三個核心步驟，以便保持清晰的結構和良好的可維護性。

此頁面展示了一個完整的 Pipeline 範例，用於說明如何利用 Kubeflow Pipelines 實現一個典型的數據處理工作流。該範例包含三個主要步驟：數據下載、數據預處理以及數據上傳。

---

## Pipeline 流程概述
### 1. 下載原始數據集（Download Original Dataset）

使用指定的憑證與檔案管理工具，從遠端伺服器下載數據集（如壓縮檔案），並將其存儲到指定的輸出目錄。
### 2. 預處理數據集（Preprocessing）

解壓下載的數據集並進行標準化處理。
數據標準化後，將其分割為序列與標籤對，並存儲為壓縮檔案格式以供後續使用。
### 3. 上傳處理後的數據集（Upload Training Dataset）

將經過預處理的數據集壓縮檔案上傳到遠端伺服器，供其他應用程序使用。

```python
import kfp
from kfp import dsl
import kfp.components as components
from kfp.components import func_to_container_op


# Define component functions
def download_original_dataset(output: components.OutputPath(), original_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
    import os
    from mitlab_aiml_tools.auth.credential import CredentialServer
    from mitlab_aiml_tools.pipeline.file import FileUtility
    # initial credential
    credential_server = CredentialServer(
        host=host,
        port=port,
        access_key=access_key,
        secret_key=secret_key,
        api_version='v1.1.4'
    )
    file_manager = FileUtility(credential_manager=credential_server,api_version='v1.1.3')

    # download original dataset
    file_type = "dataset"
    downloaded_file = file_manager.download(
        file_type=file_type, file_path=original_dataset_uid)


    # save download original dataset to output path
    download_folder_path = output
    download_file_path = f"""{output}/original_dataset.zip"""
    os.makedirs(download_folder_path, exist_ok=True)
    with open(download_file_path, "wb") as file:
        file.write(downloaded_file.content)


def preprocessing(input: components.InputPath(), output: components.OutputPath()):
    import os
    import pandas as pd
    import numpy as np
    from mitlab_aiml_tools.pipeline.compress import CompressionUtility

    # decompress original dataset in input path
    input_file_path = f"""{input}/original_dataset.zip"""
    decompress_folder_path = "./original_dataset/"
    decompress_file_path = "./original_dataset/original_dataset.csv"
    os.makedirs(decompress_folder_path, exist_ok=True)
    CompressionUtility.decompress(
        compressed_file_path=input_file_path, extract_path=decompress_folder_path)

    print(os.listdir(decompress_folder_path))

    # normalization
    data_set = pd.read_csv(decompress_file_path)
    data_set.drop("time", axis=1, inplace=True)
    data_set = data_set.to_numpy()
    std = []
    mean = []
    for i in range(3):
        std.append(np.std(data_set[:, i]))
        mean.append(np.mean(data_set[:, i]))
        data_set[:, i] = (data_set[:, i]-mean[i])/std[i]

    # separate the dataset to series and label pair
    one_set_data_count = 2500
    set_count = data_set.shape[0]//2500
    input_data_series_count = 100
    seperate_data = []
    series = []
    label = []
    for i in range(set_count):
        seperate_data.append(
            data_set[i*one_set_data_count: i*one_set_data_count+one_set_data_count])
        for j in range(one_set_data_count-input_data_series_count):
            series.append(seperate_data[i][j:j+input_data_series_count])
            label.append(seperate_data[i][j+input_data_series_count])
    series = np.array(series)
    label = np.array(label)

    # make training dataset
    training_dataset = np.array([[series[i], label[i]]
                                for i in range(len(series))], dtype=object)

    # save training dataset
    training_dataset_folder_path = "./training_dataset/"
    training_dataset_file_path = "./training_dataset/training_dataset.npy"
    os.makedirs(training_dataset_folder_path, exist_ok=True)
    np.save(training_dataset_file_path, training_dataset)

    # compress training dataset0
    compress_folder_path = output
    compress_file_path = f"""{output}/training_dataset.zip"""
    os.makedirs(compress_folder_path, exist_ok=True)
    CompressionUtility.compress(
        source_path=training_dataset_folder_path, compressed_file_path=compress_file_path)


def upload_training_dataset(input: components.InputPath(), training_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
    from mitlab_aiml_tools.auth.credential import CredentialServer
    from mitlab_aiml_tools.pipeline.file import FileUtility

    # initial credential
    credential_server = CredentialServer(
        host=host,
        port=port,
        access_key=access_key,
        secret_key=secret_key,
        api_version='v1.1.4'
    )
    file_manager = FileUtility(credential_manager=credential_server,api_version='v1.1.3')

    # upload training dataset
    upload_file_path = f"""{input}/training_dataset.zip"""

    print({f'file': open(upload_file_path, 'rb')})
    print({"file_path":training_dataset_uid})
    file_manager.upload(
        file_type="dataset",
        file={f'file': open(upload_file_path, 'rb')},
        file_path=training_dataset_uid
    )


# turn function into pipeline component(backend will process img_name_map)
download_original_dataset_op = func_to_container_op(
    download_original_dataset, base_image=img_name_map["download_original_dataset"])
preprocessing_op = func_to_container_op(
    preprocessing, base_image=img_name_map["preprocessing"])
upload_training_dataset_op = func_to_container_op(
    upload_training_dataset, base_image=img_name_map["upload_training_dataset"])


@dsl.pipeline(
    name='pipeline',
    description='pipeline example'
)
def pipeline(preprocessing_task_uid: str,original_dataset_uid: str, training_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):

    task1 = download_original_dataset_op(original_dataset_uid=original_dataset_uid,
                                         host=host, port=port, access_key=access_key, secret_key=secret_key)
    task1.set_cpu_request('0.5').set_cpu_limit('0.5')
    task1.set_memory_request('4000Mi').set_memory_limit('4000Mi')
    task1.set_caching_options(False)
    task1.set_image_pull_policy('Always')
    task1.add_pod_label(preprocessing_task_uid, 'download_original_dataset')

    task2 = preprocessing_op(input=task1.output)
    task2.set_cpu_request('0.5').set_cpu_limit('0.5')
    task2.set_memory_request('4000Mi').set_memory_limit('4000Mi')
    task2.set_caching_options(False)
    task2.set_image_pull_policy('Always')
    task2.add_pod_label(preprocessing_task_uid, 'preprocessing')

    task3 = upload_training_dataset_op(input=task2.output, training_dataset_uid=training_dataset_uid,
                                       host=host, port=port, access_key=access_key, secret_key=secret_key)
    task3.set_cpu_request('0.5').set_cpu_limit('0.5')
    task3.set_memory_request('4000Mi').set_memory_limit('4000Mi')
    task3.set_caching_options(False)
    task3.set_image_pull_policy('Always')
    task3.add_pod_label(preprocessing_task_uid, 'upload_training_dataset')


if __name__ == '__main__':
    kfp.compiler.Compiler().compile(pipeline, 'pipeline.yaml')
```

---

## 程式碼結構說明

### 1. Component 函數
* 下載數據集 (```download_original_dataset```)：
  * 初始化憑證伺服器並使用檔案管理工具下載數據集。
  * 將下載的數據集保存為壓縮檔案。
* 預處理數據集 (```preprocessing```)：
  * 解壓縮下載的數據集並對其進行標準化處理。
  * 按一定規則將數據切割為序列與標籤對，保存為壓縮檔案。
* 上傳數據集 (```upload_training_dataset```)：
  * 初始化憑證伺服器並使用檔案管理工具將壓縮後的數據集上傳到遠端伺服器。

### 2. Component 包裝
將上述函數轉換為可供Pipeline使用的組件，並指定其運行所需的容器映像（```base_image```）。

### 3. Pipeline 定義
```@dsl.pipeline``` 用於定義整個流水線：
* 輸入參數包括：
  * ```preprocessing_task_uid```：用於標記 Pod 的唯一識別符。
  * ```original_dataset_uid``` 和 ```training_dataset_uid```：數據集的唯一識別符。
  * 遠端伺服器的 ```host```、```port``` 和憑證信息。
* 定義了流水線的三個任務：
    1. 下載原始數據集 (```task1```)
    2. 預處理數據集 (```task2```)
    3. 上傳處理後的數據集 (```task3```)
* 每個任務均設定了計算資源請求與限制（CPU、記憶體）以及執行選項（例如禁用快取）。

---

## 操作說明

### 1. Pipeline 編譯
* 將程式碼編譯為 YAML 格式的Pipeline配置文件。
* 執行命令：
  ```bash
  python pipeline.py
  ```
  將生成 ```pipeline.yaml``` 文件。

### 2.Pipeline 部署
* 在 Kubeflow Pipelines UI 中上傳 ```pipeline.yaml``` 文件，並按照需求填入執行參數。

### 3.Pipeline 執行
* 選擇上傳的流水線並執行，檢查每個步驟的輸入與輸出是否正確。