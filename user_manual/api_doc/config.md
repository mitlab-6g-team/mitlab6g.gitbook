## Config管理

### 新增Preprocessing/Training/Optimization Config資訊
**請求方式**：POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/42XzWaxjE6dKA9mZ

**描述**：此 API 允許驗證成功的用戶根據提供的Config名稱、描述、資訊、 所屬Pipeline的UID，來新增Config資訊。

#### 請求參數
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Config名稱| -|
|description|否|Config描述|-|
|data|是|Config資訊|需為json格式|
|f_pipeline_uid|是|所屬Pipeline的UID|-|

#### 範例請求

Example RAW Request
```json
POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/42XzWaxjE6dKA9mZ
Content-Type: application/json

{
  "name": "rr",
  "description": "rr",
   "data": {},
  "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
}
```
Example CURL Request
```bash
curl --request POST \
  --url http://<host_ip>:<backend_entrypoint>/api/v1.1.1/entrypoint/Router/parse/42XzWaxjE6dKA9mZ \
  --header 'Content-Type: application/json' \
  --data '{
  "name": "rr",
  "description": "rr",
   "data": {"learning": 0.1},
  "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
}'
```

#### 範例回應

**成功回應**：
Statue Code:200

```json
{
  "detail": "Metadata created successfully",
  "data": {
    "uid": "f12f50b0-7392-488a-8087-fa66ad08cd5d",
    "created_time": "2024-09-18T16:29:41.675809+08:00",
    "name": "rr",
    "description": "rr",
    "data": {},
    "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
  }
}
```


### 查詢Pipeline底下的Preprocessing/Training/Optimization Config資訊
**請求方式**：POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/NVVJBLsdkIjTLB2P

**描述**：此 API 允許驗證成功的用戶根據提供 Preprocessing/Training/Optimization Config 所屬Pipeline的UID，來查詢所屬Pipeline下的所有Config資訊。

#### 請求參數
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| f_pipeline_uid| 是| 所屬Pipeline的UID| -|

#### 範例請求

Example RAW Request
```json
POST http://<host_ip>:<backend_entrypoint>/api/<backend_entrypoint>/entrypoint/Router/parse/NVVJBLsdkIjTLB2P
Content-Type: application/json

{
    "f_pipeline_uid":"37e952be-7cd8-463e-b3da-d28c47ed860c"
}
```
Example CURL Request
```bash
curl --request POST \
  --url http://<host_ip>:<backend_entrypoint>/api/v1.1.1/entrypoint/Router/parse/NVVJBLsdkIjTLB2P \
  --header 'Content-Type: application/json' \
  --data '{
    "f_pipeline_uid":"37e952be-7cd8-463e-b3da-d28c47ed860c"
}'
```

#### 範例回應

**成功回應**：
Statue Code:200

```json
{
  "detail": "Metadatas retrieved successfully",
  "data": {
			<List: Configs>
	}
}
```