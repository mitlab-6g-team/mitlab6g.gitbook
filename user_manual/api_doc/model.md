## Model管理

### 下載Model檔案
**請求方式**：POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/4AJSfvc4mn8xzy2U

**描述**：此 API 允許用戶根據提供Model的UID、Model的訪問權限下載Model。

#### 請求參數
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| uid| 是| Model的UID| -|
| access_token| 否 | Model的訪問權限|-|

#### 範例請求

Example RAW Request
```json
POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/4AJSfvc4mn8xzy2U
Content-Type: application/json

{
    "uid":"b4886a75-3e32-48ea-b70d-81273001eee9",
    "access_token":"b69607ba930439666c3bce6ed1493646ddfc8ada076e1410bc4028d69e21c05d"
}
```
Example CURL Request
```bash
curl --request POST \
--url http://<host_ip>:<backend_entrypoint>/api/v1.1.1/entrypoint/Router/parse/4AJSfvc4mn8xzy2U \
--data '{
    "uid":"b4886a75-3e32-48ea-b70d-81273001eee9",
    "access_token":"b69607ba930439666c3bce6ed1493646ddfc8ada076e1410bc4028d69e21c05d"
}' \
--output model.zip
```

#### 範例回應

**成功回應**：
Statue Code:200

```json
{
	"status":200,
	"data":{
			"detail":"File downloaded successfully"
	}
}

// File
Content-Type: application/octet-stream
Content-Disposition: attachment; filename={<model_uid>}.{<model_file_extension>}

<File: Model.zip>
```


### 查看所屬Application的Model資訊
**請求方式**：POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/4AJSfvc4mn8xzy2U

**描述**：此 API 允許驗證成功的用戶根據提供Model所屬Application的UID，來查詢所屬Application下的所有Model資訊。

#### 請求參數
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| f_application_uid| 是| 所屬Application的UID| -|

#### 範例請求

Example RAW Request
```json
POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/4AJSfvc4mn8xzy2U
Content-Type: application/json

{
	"f_application_uid":"90889d16-0d70-4816-a4d9-f2f9762a934e"
}
```
Example CURL Request
```bash
curl --request POST \
  --url http://<host_ip>:<backend_entrypoint>/api/v1.1.1/entrypoint/Router/parse/EU2X3oWVHQiEoYBi \
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
			<List: Models>
	}
}
```