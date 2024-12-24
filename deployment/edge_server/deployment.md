# Edge Server 部屬
這個指南幫助使用者設置邊緣伺服器。請確保在本頁面上已安裝所有基本環境和依賴項。

## 系統需求
邊緣伺服器為推論模型的部屬目標所在，透過Kong Gateway簡化部屬以及api流程

- 硬體資源
    
| 資源 | 大小 |
| --- | --- |
| CPU | 6 Core |
| Memory | 8GBRAM |
| Disk | 100GB |

- 版本需求
    - ubuntu 20.04

## 預設環境
部署流程會安裝一切所需套件

- Python 3.8
- Docker-ce 20.10.21

## 安裝包下載
```bash
git clone https://github.com/mitlab-6g-team/mitlab_edge_server.git
```

## 填入環境變數
.env.common.sample 

## 開始安裝
```bash
cd mitlab_edge_server
bash ./install_all.sh
```