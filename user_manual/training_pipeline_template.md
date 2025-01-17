# Training Pipeline 範例
本頁面展示了一個完整的 Training Pipeline 範例，說明如何利用 Kubeflow Pipelines 建立一個典型的模型訓練工作流。該範例包含三個主要步驟：下載訓練資料集、訓練模型、上傳訓練完成的模型。

---

## Training Pipeline 流程概述
### 1. 下載訓練資料集（Download Training Dataset）

從訓練平台下載指定的訓練資料集（壓縮檔案），並將其存儲到本地的指定目錄中。此步驟確保資料集可用於後續的模型訓練過程。
### 2.訓練模型（Training）

解壓縮下載的資料集，進行資料預處理與分割，並利用深度學習框架（如 TensorFlow）訓練模型。模型訓練完成後，將模型準確率與模型檔案保存到本地目錄中。

> 可選功能：紀錄Performance
>  * 可選擇是否要**記錄模型準確率**，若需要可將準確率存入檔案，以便後續使用。

### 3.上傳模型（Upload Model）
將訓練完成的模型檔案與模型準確率資訊上傳至訓練平台，供其他應用程式或後續流程使用。

---

## Training Pipeline 程式碼

```python
import kfp
from kfp import dsl
import kfp.components as components
from kfp.components import func_to_container_op


# Define component functions

def download_training_dataset(output: components.OutputPath(),training_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
    import os
    from mitlab_aiml_tools.auth.credential import CredentialServer
    from mitlab_aiml_tools.pipeline.file import FileUtility
    
    credential_server = CredentialServer(
        host=host,
        port=port,
        access_key=access_key,
        secret_key=secret_key
    )
    file_manager = FileUtility(credential_manager=credential_server)

    # download original dataset
    file_type = "dataset"
    downloaded_file = file_manager.download(
        file_type=file_type, file_path=training_dataset_uid)

    # save download original dataset to output path
    download_folder_path = output
    download_file_path = f"""{output}/training_dataset.zip"""
    os.makedirs(download_folder_path, exist_ok=True)
    with open(download_file_path, "wb") as file:
        file.write(downloaded_file.content)


def training(input: components.InputPath(), output: components.OutputPath(), learning_rate: float):
    import os
    from mitlab_aiml_tools.pipeline.compress import CompressionUtility
    import numpy as np
    from sklearn.model_selection import train_test_split
    from tensorflow.keras.models import Sequential
    from tensorflow.keras.layers import LSTM, Dense, Dropout
    from tensorflow.keras.optimizers import Adam

    # decompress training dataset in input path
    input_file_path = f"""{input}/training_dataset.zip"""
    decompress_folder_path = "./training_dataset/"
    decompress_file_path = "./training_dataset/training_dataset.npy"
    os.makedirs(decompress_folder_path, exist_ok=True)
    CompressionUtility.decompress(
        compressed_file_path=input_file_path, extract_path=decompress_folder_path)

    # make train and test
    training_dataset = np.load(decompress_file_path, allow_pickle=True)
    x = np.array([item[0] for item in training_dataset[:100]])
    y = np.array([item[1] for item in training_dataset[:100]])
    x_train, x_test, y_train, y_test = train_test_split(
        x, y, test_size=0.2, random_state=42)
    x_test, x_validate, y_test, y_validate = train_test_split(
        x_test, y_test, test_size=0.5, random_state=42)

    # Define the model architecture
    model = Sequential()
    model.add(LSTM(units=64, input_shape=(100, 3)))
    model.add(Dropout(0.2))
    model.add(Dense(32, activation='relu'))
    model.add(Dense(3, activation='linear'))

    # Compile the model
    adam_optimizer = Adam(learning_rate=learning_rate)
    model.compile(loss='mean_squared_error',
                  optimizer=adam_optimizer, metrics=['accuracy'])

    # Train the model on your data
    batch_size = 4
    model.fit(x_train, y_train, batch_size=batch_size, epochs=1,
              validation_data=(x_validate, y_validate))
    
    # [Optional - 想紀錄Performance請加這段 (1/2) ] save model accuracy to a file inside the output folder
    model_accuracy = model.history.history['accuracy'][-1] 
    accuracy_file_path = f"""{output}/model_accuracy.txt"""
    output_dir = os.path.dirname(accuracy_file_path)
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
        
    with open(accuracy_file_path, 'w') as f:
        f.write(f"{model_accuracy}")

    # save model
    model_folder_path = "./model/"
    mode_file_path = "./model/model.h5"
    os.makedirs(model_folder_path, exist_ok=True)
    model.save(mode_file_path)

    # compress model
    compress_folder_path = output
    compress_file_path = f"""{output}/model.zip"""
    os.makedirs(compress_folder_path, exist_ok=True)
    CompressionUtility.compress(
        source_path=model_folder_path, compressed_file_path=compress_file_path)


def upload_model(input: components.InputPath(), model_uid: str, host: str, port: str, access_key: str, secret_key: str):
    import os
    from mitlab_aiml_tools.auth.credential import CredentialServer
    from mitlab_aiml_tools.pipeline.file import FileUtility
    from mitlab_aiml_tools.pipeline.metrics import MetricUtility

    # initial credential
    credential_server = CredentialServer(
        host=host,
        port=port,
        access_key=access_key,
        secret_key=secret_key
    )

    # upload training dataset
    file_manager = FileUtility(credential_manager=credential_server)
    upload_file_path = f"""{input}/model.zip"""
    file_manager.upload(
        file_type="model",
        file={f'file': open(upload_file_path, 'rb')},
        file_path=model_uid
    )

    # [Optional - 想紀錄Performance請加這段 (2/2) ] upload model accuracy 
    accuracy_file_path = f"""{input}/model_accuracy.txt"""
    with open(accuracy_file_path, 'r') as f:
        model_accuracy = float(f.read())
    metric_manager = MetricUtility(credential_manager=credential_server)
    metric_manager.upload_accuracy(
        model_uid=model_uid,
        model_accuracy=model_accuracy
    )


download_training_dataset_op = func_to_container_op(
    download_training_dataset, base_image=img_name_map["download_training_dataset"])
training_op = func_to_container_op(
    training, base_image=img_name_map["training"])
upload_model_op = func_to_container_op(
    upload_model, base_image=img_name_map["upload_model"])


@dsl.pipeline(
    name='pipeline',
    description='pipeline example'
)
def pipeline(training_task_uid: str, training_dataset_uid: str, model_uid: str, learning_rate: float, host: str, port: str, access_key: str, secret_key: str):
    
    task1 = download_training_dataset_op(training_dataset_uid=training_dataset_uid,
                                         host=host, port=port, access_key=access_key, secret_key=secret_key)
    task1.set_cpu_request('0.5').set_cpu_limit('0.5')
    task1.set_memory_request('4000Mi').set_memory_limit('4000Mi')
    task1.set_caching_options(False)
    task1.set_image_pull_policy('Always')
    task1.add_pod_label(training_task_uid, 'download_training_dataset')

    task2 = training_op(input=task1.output, learning_rate=learning_rate)
    task2.set_cpu_request('0.5').set_cpu_limit('0.5')
    task2.set_memory_request('4000Mi').set_memory_limit('4000Mi')
    task2.set_caching_options(False)
    task2.set_image_pull_policy('Always')
    task2.add_pod_label(training_task_uid, 'training')

    task3 = upload_model_op(input=task2.output, model_uid=model_uid,
                            host=host, port=port, access_key=access_key, secret_key=secret_key)
    task3.set_cpu_request('0.5').set_cpu_limit('0.5')
    task3.set_memory_request('4000Mi').set_memory_limit('4000Mi')
    task3.set_caching_options(False)
    task3.set_image_pull_policy('Always')
    task3.add_pod_label(training_task_uid, 'upload_model')


if __name__ == '__main__':
    kfp.compiler.Compiler().compile(pipeline, 'pipeline.yaml')

```

---

## Training Pipeline 程式碼結構說明
以下為完整的 **Training Pipeline** 程式碼結構解說，詳細說明了程式碼各區塊的功能、哪些部分可以修改、哪些部分不可變更，確保讀者能夠正確理解並靈活調整 Pipeline。

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

##### Step1 : 下載訓練資料集（Download Training Dataset）
此函數負責從訓練平台下載指定的資料集，並將其存儲在本地路徑中。
函數名稱：`download_training_dataset`
* 函數名稱可更改，但需與下方的 `func_to_container_op` 綁定名稱保持一致。
* 函數參數不可變更，除非有 `config` 從前端傳入的參數。
* 建議在這邊跑模型訓練。
```python
def download_training_dataset(output: components.OutputPath(), training_dataset_uid: str, host: str, port: str, access_key: str, secret_key: str):
```

##### Step2 : 訓練模型（Training）
此函數負責解壓縮資料集、建立訓練和測試資料、定義模型結構，並進行模型訓練。
函數名稱：`training`
* 函數名稱可更改，但需與下方的 `func_to_container_op` 綁定名稱保持一致。
* 函數參數不可變更，因為它們對應 Kubeflow Pipeline 的 I/O 資料流。
> 使用者可以靈活運用 `config` 參數，用來快速調整參數。以 `learning_rate`為例，使用者只需在前端的 `config` 中帶入該參數，即可免去重新上傳 pipeline 的麻煩，僅需在前端進行更動即可。
使用 `config` 參數時，請記得在下方的 `def pipeline()` 的參數列表中帶入使用者定義的變數。

> 注意：`learning_rate` 參數需要從前端的 `config` 中以 `{"learning_rate": 0.0}` 的形式帶入。
```python
def training(input: components.InputPath(), output: components.OutputPath(), learning_rate: float):
```

<br/>

#### 3. Config內部的參數須帶入function中
在 `training`函數內部，需將 **config 內的參數** 帶入函數執行，以便動態調整模型的超參數，例如：

* `learning_rate`: 學習率
* `batch_size`: 批次大小
* `epochs`: 訓練輪數
這些參數應由 Pipeline 的使用者提供，並傳遞給 training 函數，確保訓練過程可靈活調整。

```python
def training(input: components.InputPath(), output: components.OutputPath(), learning_rate: float):
    import os
    from mitlab_aiml_tools.pipeline.compress import CompressionUtility
    import numpy as np
    from sklearn.model_selection import train_test_split
    from tensorflow.keras.models import Sequential
    from tensorflow.keras.layers import LSTM, Dense, Dropout
    from tensorflow.keras.optimizers import Adam

    # decompress training dataset in input path
    input_file_path = f"""{input}/training_dataset.zip"""
    decompress_folder_path = "./training_dataset/"
    decompress_file_path = "./training_dataset/training_dataset.npy"
    os.makedirs(decompress_folder_path, exist_ok=True)
    CompressionUtility.decompress(
        compressed_file_path=input_file_path, extract_path=decompress_folder_path)

    # make train and test
    training_dataset = np.load(decompress_file_path, allow_pickle=True)
    x = np.array([item[0] for item in training_dataset[:100]])
    y = np.array([item[1] for item in training_dataset[:100]])
    x_train, x_test, y_train, y_test = train_test_split(
        x, y, test_size=0.2, random_state=42)
    x_test, x_validate, y_test, y_validate = train_test_split(
        x_test, y_test, test_size=0.5, random_state=42)

    # 定義模型架構，並將 config 內的參數應用於模型
    model = Sequential()
    model.add(LSTM(units=64, input_shape=(100, 3)))
    model.add(Dropout(0.2))
    model.add(Dense(32, activation='relu'))
    model.add(Dense(3, activation='linear'))

    # Compile the model
    adam_optimizer = Adam(learning_rate=learning_rate)
    model.compile(loss='mean_squared_error',
                  optimizer=adam_optimizer, metrics=['accuracy'])

    # Train the model on your data
    batch_size = 4
    model.fit(x_train, y_train, batch_size=batch_size, epochs=1,
              validation_data=(x_validate, y_validate))
        
    with open(accuracy_file_path, 'w') as f:
        f.write(f"{model_accuracy}")

    # save model
    model_folder_path = "./model/"
    mode_file_path = "./model/model.h5"
    os.makedirs(model_folder_path, exist_ok=True)
    model.save(mode_file_path)

    # compress model
    compress_folder_path = output
    compress_file_path = f"""{output}/model.zip"""
    os.makedirs(compress_folder_path, exist_ok=True)
    CompressionUtility.compress(
        source_path=model_folder_path, compressed_file_path=compress_file_path)
```

<br/>

#### 4. 可選功能：紀錄 Performance
可選擇是否 **紀錄模型準確率**，若需要則可將準確率存入檔案，以便後續使用。
```python
# [Optional - 想紀錄Performance請加這段 (1/2) ] save model accuracy to a file inside the output folder
model_accuracy = model.history.history['accuracy'][-1] 
accuracy_file_path = f"""{output}/model_accuracy.txt"""
output_dir = os.path.dirname(accuracy_file_path)
if not os.path.exists(output_dir):
    os.makedirs(output_dir)

with open(accuracy_file_path, 'w') as f:
    f.write(f"{model_accuracy}")
```

<br/>

#### 5. 上傳模型（Upload Model）
此函數負責將訓練好的模型及準確度結果上傳到訓練平台。
函數名稱：`upload_model`
* 函數名稱可更改，但需與下方的 `func_to_container_op` 綁定名稱保持一致。
* 函數參數不可變更，除非有需要新增 `config` 之類從前端傳入的參數。
* 建議在這邊進行模型上傳的部分。
```python
def upload_model(input: components.InputPath(), model_uid: str, host: str, port: str, access_key: str, secret_key: str):
```

<br/>

可選功能：上傳Performance
  * 若 `model_accuracy.txt` 存在，則可選擇是否上傳模型的準確率資訊。
```python
# [Optional - 想紀錄Performance請加這段 (2/2) ] upload model accuracy 
accuracy_file_path = f"""{input}/model_accuracy.txt"""
with open(accuracy_file_path, 'r') as f:
    model_accuracy = float(f.read())
metric_manager = MetricUtility(credential_manager=credential_server)
metric_manager.upload_accuracy(
    model_uid=model_uid,
    model_accuracy=model_accuracy
)
```

<br/>

#### 6. 轉換為容器操作（Container Op）
每個函數必須透過 `func_to_container_op` 轉換為容器操作，才能納入 Pipeline。
這一段程式碼將上方的三個函數（`download_training_dataset`、`training`、`upload_model`）轉換為 **Pipeline** 的 **Task**，並指定對應的 **映像檔名稱**。
```python
download_training_dataset_op = func_to_container_op(
    download_training_dataset, base_image=img_name_map["download_training_dataset"])
training_op = func_to_container_op(
    training, base_image=img_name_map["training"])
upload_model_op = func_to_container_op(
    upload_model, base_image=img_name_map["upload_model"])
```
**注意事項**：
1. 函數名稱可自由命名，但必須在 Pipeline 定義中保持一致。
2. 映像檔名稱不可更動，因為這些映像檔對應到已建構的 Docker 容器。

<br/>

#### 7. 定義 Pipeline
最後，我們使用 `@dsl.pipeline` 註解來定義完整的 Pipeline。每個步驟（Task）按順序連接成完整的工作流程。
此函數定義了 Pipeline 的各項傳入參數與執行參數。
* pipeline 傳入參數不得修改。
* 不過，可以新增從 `config` 傳入的參數，例如 `learning_rate`。
* 使用這類從 `config` 帶入的參數時，請務必將其加入函數的參數列表中，作為自定義參數。
```python
@dsl.pipeline(
    name='pipeline',
    description='pipeline example'
)
def pipeline(training_task_uid: str, training_dataset_uid: str, model_uid: str, learning_rate: float, host: str, port: str, access_key: str, secret_key: str):
```

<br/>

#### 8. 定義 Task 執行順序
在 Pipeline 函數內，每個 Task（task1、task2、task3） 的變數名稱必須與前面定義的 Container Operation 變數名稱 保持一致。
```python
task1 = download_training_dataset_op(training_dataset_uid=training_dataset_uid,
                                     host=host, port=port, access_key=access_key, secret_key=secret_key)

task2 = training_op(input=task1.output, learning_rate=learning_rate)

task3 = upload_model_op(input=task2.output, model_uid=model_uid,
                        host=host, port=port, access_key=access_key, secret_key=secret_key)

```
**注意事項**：
1. `task1`、`task2`、`task3` 的變數名稱可以修改，但需要與前面定義的 `_op` 變數名稱保持一致。
2. **變數名稱錯誤會導致 Pipeline 無法正常執行。**

<br/>

#### 9. 編譯 Pipeline 成 YAML 文件
最後，將 Pipeline 編譯為 `pipeline.yaml`，以便部署至 Kubeflow。
```python
if __name__ == '__main__':
    kfp.compiler.Compiler().compile(pipeline, 'pipeline.yaml')
```
此程式碼段不可變更，因為它是將 Pipeline 轉換為 Kubeflow YAML 文件的必要步驟。

---