{% raw %}
<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

# Model管理

### 下載Model檔案
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/4AJSfvc4mn8xzy2U`

此 API 允許用戶根據提供Model的UID、Model的訪問權限下載Model。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| uid| 是| Model的UID| -|
| access_token| 否 | Model的訪問權限|-|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
    "uid":"b4886a75-3e32-48ea-b70d-81273001eee9",
    "access_token":"b69607ba930439666c3bce6ed1493646ddfc8ada076e1410bc4028d69e21c05d"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
--url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/4AJSfvc4mn8xzy2U \
--data '{
    "uid":"b4886a75-3e32-48ea-b70d-81273001eee9",
    "access_token":"b69607ba930439666c3bce6ed1493646ddfc8ada076e1410bc4028d69e21c05d"
}' \
--output model.zip
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
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
{% endcodetabs %}

---


### 查看所屬Application的Model資訊
<g>`POST`</g>  `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/4AJSfvc4mn8xzy2U`

此 API 允許驗證成功的用戶根據提供Model所屬Application的UID，來查詢所屬Application下的所有Model資訊。

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
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/EU2X3oWVHQiEoYBi \
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
		<List: Models>
	}
}
{% endcodetabs %}
