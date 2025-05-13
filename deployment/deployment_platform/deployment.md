# Deployment Platform部屬
此指南幫助使用者設置推論系統的部屬平台。請從我們的GitHub儲存庫獲取安裝程式！

## 系統需求

部屬平台主要以模型資料訂閱、docker image建立、系統資料存儲為主要目的

- 硬體資源
       
| 資源 | 大小 |
| --- | --- |
| CPU | 6 Core |
| Memory | 16GB RAM |
| Disk | 100GB |

- 版本需求
    - ubuntu 20.04

## 預設環境
部署流程會安裝一切所需套件

- Python 3.8
- Docker-ce 20.10.21

## 安裝包下載
```bash
git clone https://github.com/mitlab-6g-team/mitlab_deployment_platform.git
```

## 填入環境變數
- .env.common.sample
    - 當前系統最新版本對應請看下方表格
    - 規定
        - TRAINING_PF_HOST_IP
    - 自定義
        - DEPLOYMENT_PF_HOST_IP
        - EDGE_SERVER_HOST_IP
        - PORT
- .env.inference_config
    - 可選更動
- .env.postgres_config
    - 可選更動
- .env.pgadmin_config
    - 可選更動

## 連接埠說明
| 系統名稱 | 是否有固定port |
| --- | --- |
| AGENT_OPERATION | 36904 |
| METADATA_MGT | - |
| FILE_MGT | - |
| CENTRAL_CONNECTOR | - |
| INFERENCE_CONNECTOR | - |
| AUTHENTICATE_MIDDLEWARE | - |
| AGENT_USER_DASHBOARD | - |
| SYSTEM_TASK_MGT | 30303 |
| INFERENCE_TASK_MGT | 不需填寫（動態路由） |
| DATAFLOW_MGT | 30308 |

## 開始安裝
```bash
cd mitlab_deployment_platform
bash ./install_all.sh
```

## 激活 mitlab_deployment_server
- 填入所需資訊
    - 從 mitlab_deployment_server 資料夾內的 ``.env.common.sample`` 取得
        - DEPLOYMENT_PF_HOST_IP
        - CENTRAL_CONNECTOR_CONTAINER_PORT
    - 從 AI/ML Intelligent Platform 的Agent頁面取得資訊
        - AGENT_UID
        - AGENT_ACTIVATION_TOKEN
- 實際激活
```bash
curl --location 'http://<DEPLOYMENT_PF_HOST_IP>:<CENTRAL_CONNECTOR_CONTAINER_PORT>/api/v1.1.2/central_operation/AgentLifeManager/init' \
--header 'Content-Type: application/json' \
--data '{
	"agent_uid": "<AGENT_UID>",
	"agent_activation_token":"<AGENT_ACTIVATION_TOKEN>"
}'
```


