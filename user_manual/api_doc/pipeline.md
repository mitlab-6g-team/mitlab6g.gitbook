{% raw %}
<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

# Pipeline管理

### 上傳Preprocessing/Training/Optimization Pipeline檔案
<g>`POST`</g>  `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/FgdSO4ryg2UA2APl`

此 API 允許驗證成功的用戶根據提供的Pipeline名稱、描述、種類、副檔名、所屬Application的UID和Pipeline檔案，來新增Pipeline檔案及資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | multipart/form-data | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Pipeline名稱| -|
| description| 否 | Pipeline描述|-|
|type|是|Pipeline類別|original/training|
|f_application_uid|是|Pipeline所屬Application的UID|-|
|extension|是|Pipeline副檔名|zip|
|file|是|Pipeline檔案|-|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
  "name": "test pipeline",
  "description": "test pipeline description",
  "type": "preprocessing",
  "f_application_uid": "90889d16-0d70-4816-a4d9-f2f9762a934e",
  "extension": "py",
  "file":<file> // only .py file acceptable
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/FgdSO4ryg2UA2APl \
  --header 'Content-Type: multipart/form-data' \
  --form name=test \
  --form description=mitlab \
  --form type=preprocessing \
  --form f_application_uid=90889d16-0d70-4816-a4d9-f2f9762a934e \
  --form extension=py \
  --form file=@path/to/pipeline.py
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
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
{% endcodetabs %}

---


### 查詢Application底下的Preprocessing/Training/Optimization Pipeline資訊
<g>`POST`</g>  `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/VytZbpzyI9fFWkM6`

此 API 允許驗證成功的用戶根據提供 Preprocessing/Training/Optimization Pipeline 所屬Application的UID，來查詢所屬Application下的所有Pipeline資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| f_application_uid| 是| 所屬Application的UID| -|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "f_application_uid":"90889d16-0d70-4816-a4d9-f2f9762a934e"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/VytZbpzyI9fFWkM6 \
  --header 'Content-Type: application/json' \
  --data '{
    "f_application_uid":"90889d16-0d70-4816-a4d9-f2f9762a934e"
}'
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
{
  "detail": "Metadatas retrieved successfully",
  "data": {
		<List: Preprocessing Pipelines, Training Pipelines and Retrain Pipelines>
	}
}
{% endcodetabs %}
