# Image範例

在 Kubeflow Pipelines 中，每個 Pipeline 函數都對應到一個獨立的容器化環境，這些環境由 Docker Image 提供支持。通常情況下，所需的 Image 可以歸類為以下四種，分別針對不同的處理需求進行設計。

---
## 1. 基本Image

```dockerfile
FROM python:3.8.10
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
```
### 用途
* 適用於處理簡單任務，如數據下載等。
* 提供 Python 基礎環境與自定義工具套件。

---
## 2.Processing Image
適用於需要進行數據處理的步驟。

```dockerfile
FROM python:3.8.10
RUN pip install --no-cache-dir numpy
RUN pip install --no-cache-dir pandas
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
```
### 用途
* 提供數據處理相關的工具，如 numpy 和 pandas。
* 適合數據標準化、特徵提取等任務。

---
## 3.Training/Optimization/Evaluation Image
適用於模型訓練、優化和評估的步驟。

```dockerfile
FROM tensorflow/tensorflow:2.15.0-gpu
RUN apt-get update && apt-get install -y git
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
RUN pip install --no-cache-dir scikit-learn
```
### 用途
* 提供支持 GPU 的 TensorFlow 環境。
* 包含機器學習工具，如 scikit-learn。
* 適合進行模型訓練和評估的高性能計算場景。

---

## 4.特殊用途Image
適用於需要客製化功能的任務，與基本 Image 類似，但可以進一步擴展。

```dockerfile
FROM python:3.8.10
RUN pip install --no-cache-dir git+https://github.com/mitlab-6g-team/mitlab-aiml-tools.git@v1.1.1
```
### 用途
* 提供與基本 Image 相同的功能，但可根據需求進行擴展。

---
這些 Image 提供了通用且靈活的環境設置，可以根據 Pipeline 的需求進行組合或調整。通常情況下，以下場景會採用不同的 Image：

* **下載/上傳數據**：使用基本 Image。
* **數據處理**：使用 Processing Image。
* **模型訓練與評估**：使用支持 GPU 的 Training Image。
這樣的設計可以確保每個步驟的環境獨立性，同時降低維護成本。