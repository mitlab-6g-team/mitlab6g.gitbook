{% raw %}
<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

# Pipeline運行

### 運行Preprocessing Task
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/jPqyAFWh7hBKRRNK`

此 API 允許驗證成功的用戶根據提供的：

- 驗證執行 Task 的權限：`存取帳號`、`密碼`

- 建立 Task 的資訊：`名稱`、`描述`、`所屬的Pipeline的UID`

- Preprocessing所需的物件：`Original Dataset的UID`、`Original Dataset的種類`、`Config的UID`、`Build File的UID`

- Preprocessing工作完的Dataset資訊：`名稱`、`描述`、`種類`、`副檔名`

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| access_key| 是| 存取帳號| -|
| secret_key| 是 | 密碼|-|
|task_name|是|Task名稱|-|
|task_description|否|Task描述|-|
|pipeline_uid|是|所屬Pipeline的UID|zip|
|dataset_uid|是|Original Dataset UID|-|
|type|是|Original Dataset類別|project(訓練)/application(優化)|
|config_uid|是|Config UID|-|
|image_uid|是|Build File UID|sequence=download/running/upload|
|dataset_name|是|Dataset名稱|-|
|dataset_description|否|Dataset描述|-|
|dataset_type|是|Dataset種類|training/optimization|
|dataset_file_extension|是|Dataset副檔名|zip|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "access_key":"user1",
    "secret_key":"test",

    "task_name":"test_preprocessing_task",
    "task_description":"test preprocessing task",
    "pipeline_uid":"37e952be-7cd8-463e-b3da-d28c47ed860c",
  
    "dataset_uid": "f6fa9ff3-fce4-4790-b050-acb1c9f9a2ce",
    "type": "project",
    "config_uid":"f258b890-8a15-4f9a-a0e7-c29ad65f4da6",
    "image_uid" : {
        "download_uid":"b956f74f-30ab-4b3a-9619-6af34afcf4da",
        "running_uid":"ec8cb77d-a794-45f4-81fb-0c662f27a53c",
        "upload_uid":"476a2f5c-3b03-45b5-b8b1-51bb9891fd9f"
    },
 
    "dataset_name":"demo_training_dataset",
    "dataset_description":"demo_training_dataset_description",
    "dataset_type":"training",
    "dataset_file_extension": "zip"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/jPqyAFWh7hBKRRNK \
  --header 'Content-Type: application/json' \
  --data '{
    "access_key":"user1",
    "secret_key":"test",

    "task_name":"test_preprocessing_task",
    "task_description":"test preprocessing task",
    "pipeline_uid":"37e952be-7cd8-463e-b3da-d28c47ed860c",
  
    "dataset_uid": "f6fa9ff3-fce4-4790-b050-acb1c9f9a2ce",
    "foreignkey_uid":"e2454e78-5d23-4d5c-be87-ca2a3e679291",
    "type": "project",
    "config_uid":"f258b890-8a15-4f9a-a0e7-c29ad65f4da6",
    "image_uid" : {
        "download_uid":"b956f74f-30ab-4b3a-9619-6af34afcf4da",
        "running_uid":"ec8cb77d-a794-45f4-81fb-0c662f27a53c",
        "upload_uid":"476a2f5c-3b03-45b5-b8b1-51bb9891fd9f"
    },
 
    "dataset_name":"demo_training_dataset",
    "dataset_description":"demo_training_dataset_description",
    "dataset_type":"training",
    "dataset_file_extension": "zip"
}'
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
{
  "detail": "Excute task successfully",
  "data": {
    "training_dataset_uid": "4eda5599-5324-6174-8f9e-61e155207a6e"
    "uid": "169920c2-0441-4f47-82b1-41b9c859bf06",
    "created_time": "2024-09-18T18:13:22.552851+08:00",
    "name": "test_preprocessing_task",
    "description": "test preprocessing task",
    "execute_step": "prepare_file",
    "status": "init",
    "f_download_image_uid": "b956f74f-30ab-4b3a-9619-6af34afcf4da",
    "f_running_image_uid": "ec8cb77d-a794-45f4-81fb-0c662f27a53c",
    "f_upload_image_uid": "476a2f5c-3b03-45b5-b8b1-51bb9891fd9f",
    "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
  }
}
{% endcodetabs %}

---


### 運行Training Task
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/q8uzMBcM5YJH6dPf`

此 API 允許驗證成功的用戶根據提供的：

- 驗證執行 Task 的權限：`存取帳號`、`密碼`

- 建立 Task 的資訊：`名稱`、`描述`、`所屬Pipeline的UID`

- 訓練所需的物件：`Training Dataset的UID`、 `Dataset種類`、`Config的UID`、`Build File的UID`

- Training Task完成的Model資訊：`名稱`、`描述`、`種類`、`輸入格式`、`輸出格式`、`副檔名`

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| access_key| 是| 存取帳號| -|
| secret_key| 是 | 密碼|-|
|task_name|是|Task名稱|-|
|task_description|否|Task描述|-|
|pipeline_uid|是|所屬Pipeline的UID|zip|
|dataset_uid|是|Original Dataset UID|-|
|type|是|Original Dataset類別|project(訓練)|
|config_uid|是|Config UID|-|
|image_uid|是|Build File UID|sequence=download/running/upload|
|model_name|是|Model名稱|-|
|model_description|否|Model描述|-|
|model_type|是|Model種類|default|
|model_input_format|是|Model輸入格式|-|
|model_output_format|是|Model輸出格式|-|
|model_file_extension|是|Model副檔名|zip|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "access_key":"user1",
    "secret_key":"test",

    "task_name":"test_training_task",
    "task_description":"test training task",
    "pipeline_uid":"59d52295-ed5a-499c-b70e-ce42d35d765c",
  
    "dataset_uid": "148b95eb-b85e-49b8-adda-dcea5254f4b5",
    "type": "project",
    "config_uid":"9e673f02-6da0-441d-bf90-94d6a1ff6b02",
    "image_uid" : {
        "download_uid":"13d5eae7-f617-4f3d-bc87-56d7a2f2fc43",
        "running_uid":"26a9c15b-f3d5-412b-95df-693b66e5a2cb",
        "upload_uid":"3232b619-7be8-4240-9aa9-549511797063"
    },
 
    "model_name":"demo_training_dataset",
    "model_description":"demo_training_dataset_description",
    "model_type":"default",
    "model_input_format": "float32",
    "model_output_format": "float32",
    "model_file_extension": "zip"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/q8uzMBcM5YJH6dPf \
  --header 'Content-Type: application/json' \
  --data '{
    "access_key":"user1",
    "secret_key":"test",

    "task_name":"test_training_task",
    "task_description":"test training task",
    "pipeline_uid":"59d52295-ed5a-499c-b70e-ce42d35d765c",
  
    "dataset_uid": "148b95eb-b85e-49b8-adda-dcea5254f4b5",
    "type": "project",
    "config_uid":"9e673f02-6da0-441d-bf90-94d6a1ff6b02",
    "image_uid" : {
        "download_uid":"13d5eae7-f617-4f3d-bc87-56d7a2f2fc43",
        "running_uid":"26a9c15b-f3d5-412b-95df-693b66e5a2cb",
        "upload_uid":"3232b619-7be8-4240-9aa9-549511797063"
    },
 
    "model_name":"demo_training_dataset",
    "model_description":"demo_training_dataset_description",
    "model_type":"default",
    "model_input_format": "float32",
    "model_output_format": "float32",
    "model_file_extension": "zip"
}'
{% endcodetabs %}

#### 回應範例

{% codetabs name="200", type="json" -%}
{
  "detail": "Excute task successfully",
  "data": {
    "model_uid": "b4095d7e-04d0-dba6-68d4-9cbb832a4fd8"
    "uid": "8249529a-a009-40c4-b9d3-9dfc429f4c85",
    "created_time": "2024-09-18T18:45:00.772126+08:00",
    "name": "test_training_task",
    "description": "test training task",
    "execute_step": "prepare_file",
    "status": "init",
    "f_download_image_uid": "13d5eae7-f617-4f3d-bc87-56d7a2f2fc43",
    "f_running_image_uid": "26a9c15b-f3d5-412b-95df-693b66e5a2cb",
    "f_upload_image_uid": "3232b619-7be8-4240-9aa9-549511797063",
    "f_pipeline_uid": "59d52295-ed5a-499c-b70e-ce42d35d765c"
  }
}
{% endcodetabs %}

---


### 運行Optimization Task
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/Oi6u8dkur8GTKPxS`

此 API 允許驗證成功的用戶根據提供的：

- 驗證執行 Task 的權限：`存取帳號`、`密碼`

- 建立 Task 的資訊：`名稱`、`描述`、`所屬Pipeline的UID`
  
- Optimization所需的物件：`已訓練好的Model的UID`、`訓練Dataset的UID`、`Dataset種類`、`Config的UID`、`Build File的UID`

- Optimization Task完成的Model資訊：`名稱`、`描述`、`種類`、`輸入格式`、`輸出格式`、`副檔名`

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| access_key| 是| 存取帳號| -|
| secret_key| 是 | 密碼|-|
|task_name|是|Task名稱|-|
|task_description|否|Task描述|-|
|pipeline_uid|是|所屬Pipeline的UID|zip|
|model_uid|是|已訓練好的Model的UID|-|
|dataset_uid|是|訓練Dataset的UID|-|
|type|是|Original Dataset類別|application(優化)|
|config_uid|是|Config UID|-|
|image_uid|是|Build File UID|sequence=download/running/upload|
|model_name|是|Model名稱|-|
|model_description|否|Model描述|-|
|model_type|是|Model種類|default|
|model_input_format|是|Model輸入格式|-|
|model_output_format|是|Model輸出格式|-|
|model_file_extension|是|Model副檔名|zip|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "access_key":"user1",
    "secret_key":"test",

    "task_name":"test_retrain_task",
    "task_description":"test retrain task",
    "pipeline_uid":"0863f72a-2f98-45bd-8dc2-4f1ff0e551a7",
  
    "model_uid":"b4886a75-3e32-48ea-b70d-81273001eee9",
    "dataset_uid": "254d343c-0b06-4c7e-998a-59161c8ccb9e",
    "type": "application",
    "config_uid":"705d5f45-ffe5-4a57-90cf-2af9cc8d0773",
    "image_uid" : {
        "download_uid":"b7181647-18df-4467-808d-7ad62b5cdd8a",
        "running_uid":"7b662f03-5f20-41ea-8f96-04740102a53f",
        "upload_uid":"17e0fb53-c616-4e83-b1d2-5ca526326ec1"
    },
 
    "model_name":"demo_retrain_dataset",
    "model_description":"retrain model",
    "model_type":"default",
    "model_input_format": "float32",
    "model_output_format": "float32",
    "model_file_extension": "zip"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/Oi6u8dkur8GTKPxS \
  --header 'Content-Type: application/json' \
  --data '{
    "access_key":"user1",
    "secret_key":"test",

    "task_name":"test_retrain_task",
    "task_description":"test retrain task",
    "pipeline_uid":"0863f72a-2f98-45bd-8dc2-4f1ff0e551a7",
  
    "model_uid":"b4886a75-3e32-48ea-b70d-81273001eee9",
    "dataset_uid": "254d343c-0b06-4c7e-998a-59161c8ccb9e",
    "type": "application",
    "config_uid":"705d5f45-ffe5-4a57-90cf-2af9cc8d0773",
    "image_uid" : {
        "download_uid":"b7181647-18df-4467-808d-7ad62b5cdd8a",
        "running_uid":"7b662f03-5f20-41ea-8f96-04740102a53f",
        "upload_uid":"17e0fb53-c616-4e83-b1d2-5ca526326ec1"
    },
 
    "model_name":"demo_retrain_dataset",
    "model_description":"retrain model",
    "model_type":"default",
    "model_input_format": "float32",
    "model_output_format": "float32",
    "model_file_extension": "zip"
}
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
{
  "detail": "Excute task successfully",
  "data": {
    "model_uid": "b156945e-7d3e-63e3-2ddd-3307fe9be4d5"
    "uid": "122d39cf-73b4-4d73-b5e3-7c14a04dfe11",
    "created_time": "2024-09-18T18:54:07.328958+08:00",
    "name": "test_retrain_task",
    "description": "test retrain task",
    "execute_step": "prepare_file",
    "status": "init",
    "f_download_image_uid": "b7181647-18df-4467-808d-7ad62b5cdd8a",
    "f_running_image_uid": "7b662f03-5f20-41ea-8f96-04740102a53f",
    "f_upload_image_uid": "17e0fb53-c616-4e83-b1d2-5ca526326ec1",
    "f_pipeline_uid": "0863f72a-2f98-45bd-8dc2-4f1ff0e551a7"
  }
}
{% endcodetabs %}

---

### 查看單一Preprocessing/Training/Optimization Task資訊
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/KlnZtRyqy3rPAZWL`

此 API 允許驗證成功的用戶根據Task的UID來查看工作狀態。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| uid| 是| Task的UID| -|


#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "uid":"169920c2-0441-4f47-82b1-41b9c859bf06"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/KlnZtRyqy3rPAZWL \
  --header 'Content-Type: application/json' \
  --data '{
    "uid":"169920c2-0441-4f47-82b1-41b9c859bf06"
}'
{% endcodetabs %}

#### 回應範例

{% codetabs name="200", type="json" -%}
{
  "detail": "Metadata retrieved successfully",
  "data": {
    "uid": "169920c2-0441-4f47-82b1-41b9c859bf06",
    "created_time": "2024-09-18T18:13:22.552851+08:00",
    "name": "test_preprocessing_task",
    "description": "test preprocessing task",
    "execute_step": "run_task",
    "status": "running",
    "f_download_image_uid": "b956f74f-30ab-4b3a-9619-6af34afcf4da",
    "f_running_image_uid": "ec8cb77d-a794-45f4-81fb-0c662f27a53c",
    "f_upload_image_uid": "476a2f5c-3b03-45b5-b8b1-51bb9891fd9f",
    "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
  }
}
{% endcodetabs %}

---

### 查看Preprocessing/Training/Optimization Task Log
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/bJ7xLmgp4WSWK498`

此 API 允許驗證成功的用戶根據Tsk的UID、工作種類來查看Task Log。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| task_uid| 是| Task的UID| -|
|type|是|Task種類|preprocessing/training/retrain|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "task_uid":"169920c2-0441-4f47-82b1-41b9c859bf06",
    "type":"preprocessing"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/bJ7xLmgp4WSWK498 \
  --header 'Content-Type: application/json' \
  --data '{
    "task_uid":"169920c2-0441-4f47-82b1-41b9c859bf06",
    "type":"preprocessing"
}'
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
{
  "detail": "Get log successfully",
  "data": {
    "download_original_dataset": "time=\"2024-09-18T10:13:37.256Z\" level=info msg=\"capturing logs\" argo=true\ngreat\nhttp://140.118.122.164:34804/api/{{ book.entrypoint_version }}/pipeline_operation/GeneralFileManager/download\n<Response [200]>\ntime=\"2024-09-18T10:13:38.258Z\" level=info msg=\"sub-process exited\" argo=true error=\"<nil>\"\ntime=\"2024-09-18T10:13:38.258Z\" level=info msg=\"/tmp/outputs/output/data -> /var/run/argo/outputs/artifacts/tmp/outputs/output/data.tgz\" argo=true\ntime=\"2024-09-18T10:13:38.258Z\" level=info msg=\"Taring /tmp/outputs/output/data\"\ntime=\"2024-09-18T10:13:38.357Z\" level=info msg=\"archived 2 files/dirs in /tmp/outputs/output/data\"\n",
    "preprocessing": "time=\"2024-09-18T10:13:47.457Z\" level=info msg=\"capturing logs\" argo=true\ntime=\"2024-09-18T10:14:19.482Z\" level=info msg=\"sub-process exited\" argo=true error=\"<nil>\"\ntime=\"2024-09-18T10:14:19.482Z\" level=info msg=\"/tmp/outputs/output/data -> /var/run/argo/outputs/artifacts/tmp/outputs/output/data.tgz\" argo=true\ntime=\"2024-09-18T10:14:19.482Z\" level=info msg=\"Taring /tmp/outputs/output/data\"\ntime=\"2024-09-18T10:14:19.953Z\" level=info msg=\"archived 2 files/dirs in /tmp/outputs/output/data\"\n",
    "upload_training_dataset": "time=\"2024-09-18T10:14:32.469Z\" level=info msg=\"capturing logs\" argo=true\ngreat\ntime=\"2024-09-18T10:14:33.470Z\" level=info msg=\"sub-process exited\" argo=true error=\"<nil>\"\n"
  }
}
{% endcodetabs %}
