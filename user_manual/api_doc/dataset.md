{% raw %}
<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>


# Dataset管理

### 上傳訓練用的Original/Training Dataset檔案

<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/OFbaCJE62lcPVbZj`

此 API 允許驗證成功的用戶根據提供的Dataset名稱、描述、類別、副檔名、所屬Project的UID和Dataset檔案，來新增Dataset檔案及資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | multipart/form-data | -|

#### Body
  
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Dataset名稱| -|
| description| 否 | Dataset描述|-|
|type|是|Dataset類別|original/training|
|f_project_uid|是|Dataset所屬Project的UID|-|
|extension|是|Dataset副檔名|zip|
|file|是|Dataset檔案|-|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "name":"test_dataset",
    "description":"test dataset description",
    "type":"original",
    "f_project_uid":"857c1a30-6743-4f38-a01d-82248f1bba81",
    "extension": "zip",
    "file":<file> // only zip file acceptable
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}
  /api/{{ book.entrypoint_version }}/entrypoint/Router/parse/OFbaCJE62lcPVbZj \
  --header 'Content-Type: multipart/form-data' \
  --form name=test \
  --form description=mitlab \
  --form type=original \
  --form f_project_uid=e2454e78-5d23-4d5c-be87-ca2a3e679291 \
  --form extension=zip \
  --form file=@path/to/original_dataset.zip
{% endcodetabs %}

#### 回應範例

{% codetabs name="200", type="json" -%}
{
  "detail": "Metadata created and File uploaded successfully",
  "data": {
    "uid": "36283293-a784-427e-8153-9bb2b430dd26",
    "created_time": "2024-09-18T14:11:50.992286+08:00",
    "name": "test",
    "description": "mitlab",
    "type": "original",
    "f_file_uid": {
      "uid": "0363cc57-4dba-4112-9d63-824d51c729eb",
      "created_time": "2024-09-18T14:11:50.978848+08:00",
      "path": "92e65171-368c-4dab-ba95-57994c156b58/e2454e78-5d23-4d5c-be87-ca2a3e679291/original",
      "extension": "zip"
    },
    "f_project_uid": "e2454e78-5d23-4d5c-be87-ca2a3e679291"
  }
}
{% endcodetabs %}

---

### 上傳優化用的Original/Optimization Dataset檔案

<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/5fk1gVLi8q0mcHf4`

此 API 允許驗證成功的用戶根據提供的Dataset名稱、描述、類別、副檔名、所屬Application的UID和Dataset檔案，來新增Dataset檔案及資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | multipart/form-data | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Dataset名稱| -|
| description| 否 | Dataset描述|-|
|type|是|Dataset類別|original/optimization|
|f_application_uid|是|Dataset所屬Application的UID|-|
|extension|是|Dataset副檔名|zip|
|file|是|Dataset檔案|-|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "name":"test_dataset",
    "description":"test dataset description",
    "type":"original",
    "f_application_uid":"90889d16-0d70-4816-a4d9-f2f9762a934e",
    "extension": "zip",
    "file":<file> // only zip file acceptable
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/5fk1gVLi8q0mcHf4 \
  --header 'Content-Type: multipart/form-data' \
  --form name=test \
  --form description=mitlab \
  --form type=original \
  --form f_application_uid=90889d16-0d70-4816-a4d9-f2f9762a934e \
  --form extension=zip \
  --form file=@path/to/original_dataset.zip
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
{
  "detail": "Metadata created and File uploaded successfully",
  "data": {
    "uid": "a9086e43-dbbd-407b-9f5f-30ae36cf6b55",
    "created_time": "2024-09-18T14:21:26.523477+08:00",
    "name": "test",
    "description": "mitlab",
    "type": "original",
    "f_file_uid": {
      "uid": "95e5fce0-e27a-471b-9685-fd6a70f873f8",
      "created_time": "2024-09-18T14:21:26.520617+08:00",
      "path": "92e65171-368c-4dab-ba95-57994c156b58/e2454e78-5d23-4d5c-be87-ca2a3e679291/90889d16-0d70-4816-a4d9-f2f9762a934e/original",
      "extension": "zip"
    },
    "f_application_uid": "90889d16-0d70-4816-a4d9-f2f9762a934e"
  }
}
{% endcodetabs %}

---

### 查詢Project底下訓練用的Orignal/Training Dataset資訊

<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/MyC2aIHtzkZJrEGi`

此 API 允許驗證成功的用戶根據提供 訓練用的Orignal/Training Dataset 所屬Project的UID，來查詢所屬Project下的所有Orignal/Training Dataset資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| f_project_uid | 是 | 所屬Project的UID | -|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "f_project_uid":"e2454e78-5d23-4d5c-be87-ca2a3e679291"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/MyC2aIHtzkZJrEGi \
  --header 'Content-Type: application/json' \
  --data '{
    "f_project_uid":"e2454e78-5d23-4d5c-be87-ca2a3e679291"
}'
{% endcodetabs %}

#### 回應範例

{% codetabs name="200", type="json" -%}
{
  "detail": "Metadatas retrieved successfully",
  "data": {
		<List: Project Original Datasets and Training Datasets>
	}
}
{% endcodetabs %}

---

### 查詢Application底下優化用的Original/Optimization Dataset資訊

<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/1sphuopUuLnIqoPU`

此 API 允許驗證成功的用戶根據提供 優化用的Original/Optimization Dataset 所屬Application的UID，來查詢所屬Application下的所有Original/Optimization Dataset資訊。

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
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/1sphuopUuLnIqoPU \
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
		<List: Application Original Datasets and Optimization Datasets>
	}
}
{% endcodetabs %}

