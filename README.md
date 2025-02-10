# MITLab AIML Platform介紹
國立台灣科技大學 MITLab AIML Platform基於MLOps流程開發，旨在協助組織管理模型從生產訓練、部署、應用到監控與優化的全生命週期。通過 Kubernetes 叢集和控制服務分離的結構，模型開發與管理者可以快速的根據專案管理的架構，有效地訓練並部署模型至邊緣伺服器中的虛擬化資源，輕鬆串接數據來源和邊緣伺服器來取得推論服務。

在部屬文件以及使用手冊中，使用者可以根據我們的指示快速建立系統，並無負擔地實作自己的Inference template和 xApp/rApp。

## AIML Intelligent Platform

AI/ML智能平台致力於提供資料處理、模型訓練及推論服務的全方位支持。本平台透過整合多種技術，包含Docker、Kubernetes，實現模型的容器化部屬與管理，確保在不同運行環境中的一致性以及系統的高擴展性。
該平台支援xAPP及rAPP應用程式，透過消息佇列將原始資料傳送至推論主機，並接收推論的結果，實現資料的靈活處理以及模型管理。

## 部屬平台 Deployment Platform

部屬平台主要針對模型的部屬及管理，透過此平台將模型部屬到邊緣伺服器中進行推論任務，通過這個過程，使用者可以從模型訓練後直接管理實際應用的整個模型生命週期，掌握模型運行狀況，並降低部屬的難度。
該平台整合了AI/ML訓練平台的模型元數據訂閱功能，提供應用端業務邏輯管理者訂閱模型元數據，以即時掌握模型更新資訊。

### 為什麼你需要部屬平台？

- 管理模型在應用（application）中的生命週期
- 達成部屬自動化
- 支援MITLab xApp SDK


## 邊緣伺服器 Edge Server

邊緣伺服器為模型的實際部屬位置和推論操作，而協助模型處理API包裝與相關資料處理工作的系統稱之為「推論主機(Inference Host)」，接收來自應用程式的資料，並執行模型推論任務，然後將推論結果反饋給應用程式。

## 支援版本

| **部署平台版本** | **Inference Host對應版本** | **最終維護時間** |
| --- | --- | --- |
| ​v1.1 | ​v1.1 | ​- |




