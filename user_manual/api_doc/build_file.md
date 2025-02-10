{% raw %}
<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

# Build File管理

### 上傳Preprocessing/Training/Optimization Build File檔案
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/lwO7afcqdKtKEAhb`

此 API 允許驗證成功的用戶根據提供的Build File名稱、描述、序列、 所屬Pipeline的UID、副檔名和Build File檔案file，來新增Build File資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | multipart/form-data | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Build File名稱| -|
|description|否|Build File描述|-|
|sequence|是|Build File序列|download/running/upload|
|f_pipeline_uid|是|所屬Pipeline的UID|-|
|extension|是|Build File副檔名|-|
|file|是|Build File檔案|-|

#### 請求範例

{% codetabs name="Raw Request", type="json" -%}
{
  "name": "rr",
  "description": "rr",
  "sequence": "download",
  "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c",
  "extension": "null",
  "file":<file> 
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/lwO7afcqdKtKEAhb \
  --header 'Content-Type: multipart/form-data' \
  --form name=rr \
  --form description=rr \
  --form sequence=download \
  --form f_pipeline_uid=37e952be-7cd8-463e-b3da-d28c47ed860c \
  --form extension=null \
  --form file=@path/to/Dockerfile
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
{
  "detail": "Metadata created and File uploaded successfully",
  "data": {
    "uid": "b956f74f-30ab-4b3a-9619-6af34afcf4da",
    "created_time": "2024-09-18T15:29:13.675082+08:00",
    "name": "rr",
    "description": "rr",
    "sequence": "download",
    "f_file_uid": {
      "uid": "69046837-a6fd-47ef-b6f6-adeb80b64442",
      "created_time": "2024-09-18T15:29:13.672338+08:00",
      "path": "92e65171-368c-4dab-ba95-57994c156b58/e2454e78-5d23-4d5c-be87-ca2a3e679291/90889d16-0d70-4816-a4d9-f2f9762a934e/preprocessing/image",
      "extension": "null"
    },
    "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
  }
}
{% endcodetabs %}


---


### 查詢Pipeline底下的Preprocessing/Training/Optimization Build File資訊
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/eu4oNOb8E0KVaOdo`

此 API 允許驗證成功的用戶根據提供 Preprocessing/Training/Optimization Build File 所屬Pipeline的UID，來查詢所屬Pipeline下的所有Build File資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| f_pipeline_uid| 是| 所屬Pipeline的UID| -|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "f_pipeline_uid":"37e952be-7cd8-463e-b3da-d28c47ed860c"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/eu4oNOb8E0KVaOdo \
  --header 'Content-Type: application/json' \
  --data '{
    "f_pipeline_uid":"37e952be-7cd8-463e-b3da-d28c47ed860c"
}'
{% endcodetabs %}

#### 回應範例

{% codetabs name="200", type="json" -%}
{
  "detail": "Metadatas retrieved successfully",
  "data": {
		<List: Download Dockerfiles, Running Dockerfiles and Upload Dockerfiles>
	}
}
{% endcodetabs %}
