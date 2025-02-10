# API 文件

## 簡介
這份文件詳細說明了我們的 API，描述了如何與各個端點進行互動。

## API Server
| Host Name | IP | DNS | Description |
| --- | --- | --- | --- |
| MITLab Host | {{ book.mitlab_host }} | - | 需要使用學校VPN才可連通 |
| **Server Name** | **Port** | **Version** | **Description** |
| AI/ML Dashboard | {{ book.dashboard_port }} | v1.1.8 | AI/ML前端網站入口 |
| Backend Entrypoint | {{ book.entrypoint_port }} | {{ book.entrypoint_version }} | 後端操作Entrypoint |