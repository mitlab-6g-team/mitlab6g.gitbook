# Pipeline範例
Pipeline 是一種高度靈活且通用的框架，可以根據需求增減步驟。例如，當處理流程較為簡單時，可以減少步驟；而當需求更複雜時，可以添加更多任務。然而，實際應用中，資料處理的 Pipeline 通常會被設計為三個核心步驟，以便保持清晰的結構和良好的可維護性。

此頁面展示了一個完整的 Preprocessing Pipeline 範例，用於說明如何利用 Kubeflow Pipelines 實現一個典型的資料處理工作流。該範例包含三個主要步驟：資料下載、資料預處理以及資料上傳。

---

## Preprocessing Pipeline 流程概述
### 1. 下載原始資料集（Download Original Dataset）

使用指定的憑證與檔案管理工具，從訓練平台下載資料集（如壓縮檔案），並將其存儲到指定的輸出目錄。
### 2. 預處理資料集（Preprocessing）

解壓下載的資料集並進行標準化處理。
資料標準化後，將其分割為序列與標籤對，並存儲為壓縮檔案格式以供後續使用。
### 3. 上傳處理後的資料集（Upload Training Dataset）

將經過預處理的資料集壓縮檔案上傳到訓練平台，供其他應用程序使用。

---

## Preprocessing Pipeline 程式碼

```python
# Kubeflow Pipeline套件宣告，不可變更
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
        secret_key=secret_key
    )
    file_manager = FileUtility(credential_manager=credential_server, host=host, api_version='v1.1.8')

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
        secret_key=secret_key
    )
    file_manager = FileUtility(credential_manager=credential_server, host=host, api_version='v1.1.8')

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

## Preprocessing Pipeline 程式碼結構說明
以下為完整的 **Preprocessing Pipeline** 程式碼結構解說，詳細說明了程式碼各區塊的功能、哪些部分可以修改、哪些部分不可變更，確保讀者能夠正確理解並靈活調整 Pipeline。

#### 1.引入 Kubeflow Pipelines 套件
在程式碼的開頭，我們引入了必需的 **Kubeflow Pipelines (kfp)** 模組，這些模組負責 **Pipeline** 的定義與處理。
```python
import kfp
from kfp import dsl
import kfp.components as components
from kfp.components import func_to_container_op
```
這部分的 **套件宣告** 是 **不可變更** 的，因為它是 Kubeflow Pipeline 的基本框架。如果缺少這些套件，Pipeline 將無法正常運作。

<br/>

#### 2. 定義 Pipeline 組件（Component Functions）
Pipeline 的核心是由三個步驟組成的組件（Components），每個步驟的功能以函數形式實現，並透過 `func_to_container_op` 轉換成容器操作（Container Op）。每個函數的結構如下：

##### Step1 : 下載原始資料集（Download Original Dataset）
此步驟的目標是從訓練平台下載指定的資料集，並將其存儲在指定的輸出路徑。

函數名稱：`download_original_dataset`
* 函數參數不可變更，除非有需要在前端加入 `config` 的參數。
* 函數參數不可變更，因為它們對應 Kubeflow Pipeline 的 I/O 資料流。
* 建議使用此函數來下載原始資料集。
```python
def download_original_dataset(output: components.OutputPath(), original_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
```

##### Step2 : 預處理資料集（Preprocessing）
此步驟負責解壓縮資料集、標準化資料，並將其分割為序列與標籤對，然後存儲為壓縮檔案格式。
函數名稱：`preprocessing`
* 函數名稱可更改，但需與下方的 `func_to_container_op` 綁定名稱保持一致。
* 函數參數不可變更，除非有需要使用 `config` 傳入的參數才可新增。
* 建議在這裡完成原始資料的處理，並輸出訓練資料。
```python
def preprocessing(input: components.InputPath(), output: components.OutputPath()):
```

##### Step3 : 上傳處理後的資料集（Upload Training Dataset）
此步驟將預處理後的資料集上傳至訓練平台，供其他應用程序使用。
函數名稱：`upload_training_dataset`
* 函數名稱可更改，但需與下方的 `func_to_container_op` 綁定名稱保持一致。
* 函數參數不可變更，除非要使用 `config` 或其他由前端傳入的參數才可新增。
* 建議使用此函數來上傳訓練資料。
```python
def upload_training_dataset(input: components.InputPath(), training_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
```

<br/>

#### 3. 轉換為容器操作（Container Op）
每個函數必須透過 `func_to_container_op` 轉換為容器操作，才能納入 Pipeline。
這一段程式碼將上方的三個函數（`download_original_dataset`、`preprocessing`、`upload_training_dataset`）轉換為 **Pipeline** 的 **Task**，並指定對應的 **映像檔名稱**。
```python
download_original_dataset_op = func_to_container_op(
    download_original_dataset, base_image=img_name_map["download_original_dataset"])
preprocessing_op = func_to_container_op(
    preprocessing, base_image=img_name_map["preprocessing"])
upload_training_dataset_op = func_to_container_op(
    upload_training_dataset, base_image=img_name_map["upload_training_dataset"])
```
**注意事項**：
1. 函數名稱可自由命名，但必須在 Pipeline 定義中保持一致。
2. 映像檔名稱不可更動，因為這些映像檔對應到已建構的 Docker 容器。

<br/>

#### 4. 定義 Pipeline
最後，我們使用 `@dsl.pipeline` 註解來定義完整的 Pipeline。每個步驟（Task）按順序連接成完整的工作流程。
* Pipeline 傳入參數不可更改，除了新增由前端用 config 傳入的參數以外。
```python
@dsl.pipeline(
    name='pipeline',
    description='pipeline example'
)
def pipeline(preprocessing_task_uid: str, original_dataset_uid: str, training_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
```

<br/>

#### 5. 定義 Task 執行順序
在 Pipeline 函數內，每個 Task（task1、task2、task3） 的變數名稱必須與前面定義的 Container Operation 變數名稱 保持一致。
```python
task1 = download_original_dataset_op(original_dataset_uid=original_dataset_uid,
                                         host=host, port=port, access_key=access_key, secret_key=secret_key)

task2 = preprocessing_op(input=task1.output)

task3 = upload_training_dataset_op(input=task2.output, training_dataset_uid=training_dataset_uid,
                                       host=host, port=port, access_key=access_key, secret_key=secret_key)

```
**注意事項**：
1. `task1`、`task2`、`task3` 的變數名稱可以修改，但需要與前面定義的 `_op` 變數名稱保持一致。
2. **變數名稱錯誤會導致 Pipeline 無法正常執行。**

<br/>

##### 6. 編譯 Pipeline 成 YAML 文件
最後，將 Pipeline 編譯為 `pipeline.yaml`，以便部署至 Kubeflow。
```python
if __name__ == '__main__':
    kfp.compiler.Compiler().compile(pipeline, 'pipeline.yaml')
```
此程式碼段不可變更，因為它是將 Pipeline 轉換為 Kubeflow YAML 文件的必要步驟。

---