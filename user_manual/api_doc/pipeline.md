## Pipeline管理

### 上傳Preprocessing/Training/Optimization Pipeline檔案
**請求方式**：POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/FgdSO4ryg2UA2APl

**描述**：此 API 允許驗證成功的用戶根據提供的Pipeline名稱、描述、種類、副檔名、所屬Application的UID和Pipeline檔案，來新增Pipeline檔案及資訊。

#### 請求參數
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Pipeline名稱| -|
| description| 否 | Pipeline描述|-|
|type|是|Pipeline類別|original/training|
|f_application_uid|是|Pipeline所屬Application的UID|-|
|extension|是|Pipeline副檔名|zip|
|file|是|Pipeline檔案|-|

#### 範例請求

Example RAW Request
```json
POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/FgdSO4ryg2UA2APl
Content-Type: multipart/form-data

{
  "name": "test pipeline",
  "description": "test pipeline description",
  "type": "preprocessing",
  "f_application_uid": "90889d16-0d70-4816-a4d9-f2f9762a934e",
  "extension": "py",
  "file":<file> // only .py file acceptable
}
```
Example CURL Request
```bash
curl --request POST \
  --url http://<host_ip>:<backend_entrypoint>/api/v1.1.1/entrypoint/Router/parse/FgdSO4ryg2UA2APl \
  --header 'Content-Type: multipart/form-data' \
  --form name=test \
  --form description=mitlab \
  --form type=preprocessing \
  --form f_application_uid=90889d16-0d70-4816-a4d9-f2f9762a934e \
  --form extension=py \
  --form file=@path/to/pipeline.py
```

#### 範例回應

**成功回應**：
Statue Code:200

```json
{
  "detail": "Metadata created and File uploaded successfully",
  "data": {
    "uid": "69f0ce7a-1f99-4177-a269-ee5e2686fe0f",
    "created_time": "2024-09-18T15:16:42.949869+08:00",
    "name": "test",
    "description": "description",
    "type": "preprocessing",
    "f_file_uid": {
      "uid": "7e884188-305d-494f-a1a5-ff3ec9599f4c",
      "created_time": "2024-09-18T15:16:42.946023+08:00",
      "path": "92e65171-368c-4dab-ba95-57994c156b58/e2454e78-5d23-4d5c-be87-ca2a3e679291/90889d16-0d70-4816-a4d9-f2f9762a934e/preprocessing",
      "extension": "py"
    },
    "f_application_uid": "90889d16-0d70-4816-a4d9-f2f9762a934e"
  }
}
```


### 查詢Application底下的Preprocessing/Training/Optimization Pipeline資訊
**請求方式**：POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/VytZbpzyI9fFWkM6

**描述**：此 API 允許驗證成功的用戶根據提供 Preprocessing/Training/Optimization Pipeline 所屬Application的UID，來查詢所屬Application下的所有Pipeline資訊。

#### 請求參數
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| f_application_uid| 是| 所屬Application的UID| -|

#### 範例請求

Example RAW Request
```json
POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/VytZbpzyI9fFWkM6
Content-Type: application/json

{
    "f_application_uid":"90889d16-0d70-4816-a4d9-f2f9762a934e"
}
```
Example CURL Request
```bash
curl --request POST \
  --url http://<host_ip>:<backend_entrypoint>/api/v1.1.1/entrypoint/Router/parse/VytZbpzyI9fFWkM6 \
  --header 'Content-Type: application/json' \
  --data '{
    "f_application_uid":"90889d16-0d70-4816-a4d9-f2f9762a934e"
}'
```

#### 範例回應

**成功回應**：
Statue Code:200

```json
{
  "detail": "Metadatas retrieved successfully",
  "data": {
			<List: Preprocessing Pipelines, Training Pipelines and Retrain Pipelines>
	}
}
```