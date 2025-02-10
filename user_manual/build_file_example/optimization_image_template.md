# Optimization Pipeline 專用 Image 範例

在 Kubeflow Pipelines 中，每個 Pipeline 函數都對應到一個獨立的容器化環境，這些環境由 Docker Image 提供支持。通常情況下，所需的 Image 可以歸類為以下四種，分別針對不同的處理需求進行設計。

---
## 1. 原始數據下載 Image（Download Dataset Image）

```dockerfile
FROM python:3.8.10
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
```
### 用途
* 為 Pipeline 的第一步（數據下載） 提供執行環境。
* 提供 Python 基礎環境，支持數據下載和文件操作。

---
## 2.數據處理 Image（Data Processing Image）

```dockerfile
FROM python:3.8.10
RUN pip install --no-cache-dir numpy
RUN pip install --no-cache-dir pandas
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
```
### 用途
* 為 Pipeline 的第二步（數據預處理） 提供執行環境。
* 包含數據處理常用工具（如 ```numpy``` 和 ```pandas```），適合進行數據標準化、特徵提取等任務。

---
## 3.模型訓練與評估 Image（Training/Optimization/Evaluation Image）

```dockerfile
FROM tensorflow/tensorflow:2.15.0-gpu
RUN apt-get update && apt-get install -y git
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
RUN pip install --no-cache-dir scikit-learn
```
### 用途
* 適用於 模型訓練、優化和評估步驟。
* 提供支持 GPU 的 TensorFlow 環境。
* 包含機器學習工具（如 ```scikit-learn```），用於高性能計算。

---

## 4.處理後數據上傳 Image（Upload Processed Dataset Image）

```dockerfile
FROM python:3.8.10
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
```
### 用途
* 適用於 Pipeline 的第三步（數據上傳）。
* 提供文件管理相關工具，用於將處理後的數據集上傳到遠端伺服器。

---
這些 Image 提供了通用且靈活的環境設置，可以根據 Pipeline 的需求進行組合或調整。通常情況下，以下場景會採用不同的 Image：

* **下載/上傳數據**：使用基本 Image。
* **數據處理**：使用 Processing Image。
* **模型訓練與評估**：使用支持 GPU 的 Training Image。
這樣的設計可以確保每個步驟的環境獨立性，同時降低維護成本。