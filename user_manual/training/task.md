# 查看所有Training Task

## 用途

查看你的 Training Pipeline 內的所有 Task 資訊

## 操作步驟

1. 進入Training Pipeline頁面後，點擊Pipeline，即可顯示所有Task
    
    ![retrieve](../images/training/config/retreive.png)

# 新增Training Task

## 用途

為你的 Training Task 創建資訊

## 操作步驟
    
1. 按下Run Training Task按鈕
    
    ![retrieve1](../images/training/config/retrieve1.png)
    
2. 輸入你的 Task資訊，輸入完後按下Create按鈕
   1. Authentication：Access Key為登入帳號、Secret Key為登入密碼
   2. Input：選擇你要Training的Dataset、Build File(Image)、Config
   3. Output：輸入訓練完的Model資訊
   4. Task：輸入你的Task資訊
    
    ![create](../images/training/task/create.png)

3. Preprocessing Task 創建成功

    ![creat1](../images/training/task/create1.png)


# 更新Training Task

## 用途

更新你的 Training Task 資訊

## 操作步驟

1. 點擊右方的Edit圖示
    
    ![edit](../images/training/task/edit.png)
    
2. 輸入更新的Task資訊，完成後按下Update按鈕
    
    ![edit1](../images/training/task/edit1.png)
    
3. Task更新成功
    
    ![edit2](../images/training/task/edit2.png)


# 下載Training Task Log

## 用途

下載你的 Training Task Log 資訊

## 操作步驟

1. 點擊右方的Log按鈕，在下載紀錄按下保留檔案，即可下載成功
    
    ![edit2](../images/preprocessing/task/edit2.png)
    

# 刪除Training Task

## 用途

刪除已不需要的 Training Task 資訊

## 操作步驟

1. 點擊右方的Delete圖示 (當task狀態不是Running時出現)
    
    ![delete](../images/preprocessing/task/delete.png)

2. 按下Delete按鈕即可刪除成功

    ![delete1](../images/preprocessing/task/delete1.png)
