{% raw %}
<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>
# Config管理

### 新增Preprocessing/Training/Optimization Config資訊
<g>`POST`</g> `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/42XzWaxjE6dKA9mZ`

此 API 允許驗證成功的用戶根據提供的Config名稱、描述、資訊、 所屬Pipeline的UID，來新增Config資訊。

#### Headers
| Name | Value | Note |
| --------- | ---------------- |-|
| Content-Type | application/json | -|

#### Body
| 參數      | 必填 | 描述             |備註|
| --------- | ---- | ---------------- |-|
| name| 是| Config名稱| -|
|description|否|Config描述|-|
|data|是|Config資訊|需為json格式|
|f_pipeline_uid|是|所屬Pipeline的UID|-|

#### 請求範例
{% codetabs name="Raw Request", type="json" -%}
{
  "name": "rr",
  "description": "rr",
   "data": {},
  "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
}
{%- language name="cURL Request", type="bash" -%}
curl --request POST \
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/v1.1.1/entrypoint/Router/parse/42XzWaxjE6dKA9mZ \
  --header 'Content-Type: application/json' \
  --data '{
  "name": "rr",
  "description": "rr",
   "data": {"learning": 0.1},
  "f_pipeline_uid": "37e952be-7cd8-463e-b3da-d28c47ed860c"
}'
{% endcodetabs %}

#### 回應範例
{% codetabs name="200", type="json" -%}
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
{% endcodetabs %}


### 查詢Pipeline底下的Preprocessing/Training/Optimization Config資訊
<g>`POST`</g>  `http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/NVVJBLsdkIjTLB2P`

此 API 允許驗證成功的用戶根據提供 Preprocessing/Training/Optimization Config 所屬Pipeline的UID，來查詢所屬Pipeline下的所有Config資訊。

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
  --url http://{{ book.mitlab_host }}:{{ book.entrypoint_port }}/api/{{ book.entrypoint_version }}/entrypoint/Router/parse/NVVJBLsdkIjTLB2P \
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
	  <List: Configs>
	}
}
{% endcodetabs %}