#  Mitlab AI/ML Tool
此SDK是由MITLAB開發的一款創新的工具，專為協助使用者建構AI/ML模型訓練的Pipeline而設計。這個SDK提供了多種功能，包括文件存取、壓縮工具、日誌轉換等，旨在簡化開發的過程。
&nbsp;
透過強調簡單性與便利性，此SDK提升了使用者體驗，使其成為優化AI/ML模型訓練工作流程的重要資源，是現代AI/ML開發需求的強大解決方案。

---

## 安裝方式

```bash
pip install git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git
```

---

## 開始使用

### 功能總覽
以下是 MITLab AI/ML Tool 提供的核心功能：

* **Compress**：文件壓縮工具
* **Decompress**:文件解壓縮工具
* **File Access**：文件的上傳與下載（需要身份驗證）
* **Model Metric**：模型相關指標的上傳
  
---

:::
## 使用指南

### 1.Compress
將檔案壓縮為壓縮檔並存放至指定的壓縮檔路徑。

##### 參數說明：
- **source_path**：需要壓縮的原始檔案路徑

- **compressed_file_path**：壓縮後的檔案輸出路徑


```python
from mitlab_aiml_tools.pipeline.compress import CompressionUtility

SOURCE_FOLDER_PATH = './tests/dataset'
COMPRESSED_FILE_PATH = './tests/compressed_dataset.zip'

# Compress files
CompressionUtility.compress(source_path=SOURCE_FOLDER_PATH,compressed_file_path=COMPRESSED_FILE_PATH)
```

---

### 2. Decompress
將壓縮檔解壓縮至***指定的路徑***。

##### 參數說明：
- **compressed_file_path**：要解壓縮的壓縮檔路徑

- **extract_path**：解壓縮後的檔案輸出路徑

```python
from mitlab_aiml_tools.pipeline.compress import CompressionUtility

COMPRESSED_FILE_PATH = './tests/compressed_dataset.zip'
EXTRACT_PATH = './tests/decompressed_dataset'

#Decompress file
CompressionUtility.decompress(compressed_file_path=COMPRESSED_FILE_PATH,extract_path=EXTRACT_PATH)
```

---

### 3.File Access (Authentication Required)
此工具支持從 MITLab AI/ML 文件伺服器上傳和下載文件。需要先完成身份驗證。

#### Step1. Authentication
認證至憑證伺服器，以取得 SDK 的存取憑證。

##### 參數說明：
- **host**：憑證伺服器的主機 IP

- **port**：憑證伺服器的服務端口

- **access_key**：MITLab AI/ML 平台的帳號

- **secret_key**：MITLab AI/ML 平台的密碼

```python
# Example args
FILE_TYPE = 'model'
model_uid= 'c876739d-9242-4aff-b800-9930c205d3c9'

# Load file
with open(MODEL_PATH, 'rb') as file:
	files = {'file':file}

# Upload file
file_manager.upload(
        file_type=FILE_TYPE,
        file_path=model_uid,
        file=files
)
```

#### Step2. Usage

* ##### Upload(上傳)
將文件上傳至 MITLab AI/ML 文件伺服器。

##### 參數說明：
- **file_type（str）**：文件類型，例如 model、dataset

- **file_path（str）**：文件的 UID

- **file（dict）**：需要上傳的文件

```python
# Example args
FILE_TYPE = 'model'
model_uid= 'c876739d-9242-4aff-b800-9930c205d3c9'

# Load file
with open(MODEL_PATH, 'rb') as file:
	files = {'file':file}

# Upload file
file_manager.upload(
        file_type=FILE_TYPE,
        file_path=model_uid,
        file=files
)
```

* ##### Download(下載)
從 MITLab AI/ML 文件伺服器下載文件。

##### 參數說明：
- **file_type**：文件類型，例如 model、dataset

- **uid**：文件的 UID

**Dataset**

```python
# Example args
FILE_TYPE = 'dataset'
MODEL_DOWNLOAD_EXAMPLE_UID = 'c876739d-9242-4aff-b800-9930c205d3c9'
DATASET_PATH = 'original_dataset.zip'

# Download file
download_file = file_manager.download(
	file_type=FILE_TYPE,
	file_path=DATASET_PATH 
)

# Save file
with open(MODEL_PATH, 'wb') as file:
            file.write(download_file.content)
```

**Model**

```python
# Example args
model_uid= 'c876739d-9242-4aff-b800-9930c205d3c9'
model_access_token= 'c876739d-9242-4aff-b800-9930c205d3c9'
MODEL_PATH = 'model.zip'

# Download file
download_file = file_manager.download(
	file_type='model',
	model_uid=model_uid,
	model_access_token=model_access_token
)

# Save file
with open(MODEL_PATH, 'wb') as file:
            file.write(download_file.content)
```

---

### 4.Model Metric (Authentication Required)
此功能可用於將模型的**相關指標**上傳至 MITLab AI/ML 平台。

##### 參數說明：
- **host**：身份驗證伺服器的 IP 地址

- **port**：身份驗證服務的端口

- **access_key**：身份驗證伺服器的訪問金鑰（AI/ML 平台的帳號）

- **secret_key**：身份驗證伺服器的密鑰（AI/ML 平台的密碼）

```python
from mitlab_aiml_tools.auth.credential import CredentialServer
from mitlab_aiml_tools.pipeline.metrics import MetricUtility

credential_server = CredentialServer(
    host=<SERVER_HOST>,
    port=<SERVER_PORT>,
    access_key=<CREDENTIAL_ACCESS_KEY>,
    secret_key=<CREDENTIAL_SECRET_KEY>
)

metric_manager = MetricUtility(credential_manager=credential_server)
 
response = metric_manager.upload_accuracy(
          model_uid=<model_uid>,
          model_accuracy=<model_accuracy>,
      )
```