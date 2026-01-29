# 整體使用流程
本頁面提供了執行各類任務（包括 Preprocessing、Training、Optimization 以及 Evaluation）的完整操作指引，幫助用戶熟悉從創建專案到執行任務的每一步操作。每個流程皆包含必要的步驟說明與對應的介面截圖，便於快速理解和實踐。

## 如何執行一個 Preprocessing Task

1. **創建Project**

   >在Project頁面點擊Create Project，輸入資訊後即可創建一個Project

    ![1-1](images/task_flow/preprocessing/1-1.png)

2. **創建Dataset**
   >這裡的Dataset有分為兩個類別：
   >Original Dataset：進入Dataset頁面，點擊Upload Original Dataset，輸入資訊後即可上傳一個dataset

    ![2-1](images/task_flow/preprocessing/2-1.png)

    ![3-1](images/task_flow/preprocessing/3-1.png)

   >Optimization Dataset：在Application Dashboard頁面選擇Optimization Dataset，點擊Upload Original Dataset，輸入資訊後即可上傳一個dataset

    ![5-1](images/task_flow/preprocessing/5-1.png)

    ![3-2](images/task_flow/preprocessing/3-2.png)

3. **創建Applicaiton**
   >回到Project頁面，點擊Application按鈕

    ![4-1](images/task_flow/preprocessing/2-1.png)

   >在Application頁面點擊Create Application，輸入資訊後即可創建一個Application
   
    ![4-2](images/task_flow/preprocessing/4-2.png)

4. **創建Preprocessing Pipeline**
   >在Application Dashboard頁面點擊Preprocessing Pipeline

    ![5-1](images/task_flow/preprocessing/5-1.png)

   >在Preprocessing Pipeline頁面點擊Create Pipeline，輸入資訊後即可創建一個Pipeline

    ![5-2](images/task_flow/preprocessing/5-2.png)

5. **創建Preprocessing Config**
   >點擊Pipeline後進入Pipeline頁面，點擊Config按鈕

    ![6-1](images/task_flow/preprocessing/6-1.png)

   >點擊Create Task Config，輸入資訊後即可創建一個Config

    ![6-2](images/task_flow/preprocessing/6-2.png)

6.  **創建Preprocessing Image**
    >點擊Build File按鈕

    ![7-1](images/task_flow/preprocessing/7-1.png)

    >點擊Upload Build File，分別輸入三個Image資訊後，即可創建完run task所需的所有Image

    ![7-2](images/task_flow/preprocessing/7-2.png)

7.  **創建Preprocessing Task**
    >點擊Tasks按鈕

    ![8-1](images/task_flow/preprocessing/8-1.png)

    >點擊Run Preprocessing Task按鈕，選擇要執行的Dataset Type、Dataset、Image、Config，及其他資訊後，按下Create按鈕

    ![8-2](images/task_flow/preprocessing/8-2.png)


## 如何執行一個Training Task

1. **創建Project**
    >在Project頁面點擊Create Project，輸入資訊後即可創建一個Project

    ![1-1](images/task_flow/preprocessing/1-1.png)

2. **創建Dataset**
    >進入Dataset頁面，點擊Training Datasets，再點擊Upload Training Dataset按鈕，輸入資訊後即可上傳一個dataset

    ![2-1](images/task_flow/training/2-1.png)

3. **創建Applicaiton**
   >回到Project頁面，點擊Application按鈕

    ![4-1](images/task_flow/preprocessing/2-1.png)

   >在Application頁面點擊Create Application，輸入資訊後即可創建一個Application
   
    ![4-2](images/task_flow/preprocessing/4-2.png)

4. **創建Training Pipeline**
   >在Application Dashboard頁面點擊Training Pipeline

    ![5-1](images/task_flow/preprocessing/5-1.png)

   >在Preprocessing Pipeline頁面點擊Create Pipeline，輸入資訊後即可創建一個Pipeline

    ![5-2](images/task_flow/training/5-2.png)

5. **創建Training Config**
    >點擊Pipeline後進入Pipeline頁面，點擊Config按鈕

    ![6-1](images/task_flow/training/6-1.png)

   >點擊Create Task Config，輸入資訊後即可創建一個Config

    ![6-2](images/task_flow/training/6-2.png)

6. **創建Training Image**
    >點擊Build File按鈕

    ![7-1](images/task_flow/training/7-1.png)

    >點擊Upload Build File，分別輸入三個Image資訊後，即可創建完run task所需的所有Image

    ![7-2](images/task_flow/training/7-2.png)

7. **創建Training Task**
    >點擊Tasks按鈕

    ![8-1](images/task_flow/training/8-1.png)

    >點擊Run Training Task按鈕，選擇要執行的Dataset、Image、Config，及其他資訊後，按下Create按鈕

    ![8-2](images/task_flow/training/8-2.png)


## 如何執行一個Optimization Task

1. **創建Project**
    >在Project頁面點擊Create Project，輸入資訊後即可創建一個Project

    ![1-1](images/task_flow/preprocessing/1-1.png)

2. **創建Applicaiton**
   >回到Project頁面，點擊Application按鈕

    ![4-1](images/task_flow/preprocessing/2-1.png)

   >在Application頁面點擊Create Application，輸入資訊後即可創建一個Application
   
    ![4-2](images/task_flow/preprocessing/4-2.png)

3. **創建Dataset**
    >進入Application Dashboard頁面，點擊Optimization Datasets

    ![5-1](images/task_flow/preprocessing/5-1.png)

    >選取Optimization Datasets，點擊Upload Optimizaion Dataset按鈕，輸入資訊後即可上傳一個dataset

    ![3-2](images/task_flow/optimization/3-2.png)

4. **創建Model**
    >回到Application Dashboard頁面，點擊Model

    ![5-1](images/task_flow/preprocessing/5-1.png)

    >點擊Upload Model，輸入資訊後即可創建一個Model

    ![4-1](images/task_flow/optimization/4-1.png)

5. **創建Optimization Pipeline**
   >回到Application Dashboard頁面，點擊Optimization Pipeline

    ![5-1](images/task_flow/preprocessing/5-1.png)

   >在Preprocessing Pipeline頁面點擊Create Pipeline，輸入資訊後即可創建一個Pipeline

    ![4-2](images/task_flow/optimization/4-2.png)

6. **創建Optimization Config**
    >點擊Pipeline後進入Pipeline頁面，點擊Config按鈕

    ![5-1](images/task_flow/optimization/5-1.png)

   >點擊Create Task Config，輸入資訊後即可創建一個Config

    ![5-2](images/task_flow/optimization/5-2.png)

7. **創建Optimization Image**
    >點擊Build File按鈕

    ![6-1](images/task_flow/optimization/6-1.png)

    >點擊Upload Build File，分別輸入三個Image資訊後，即可創建完run task所需的所有Image

    ![6-2](images/task_flow/optimization/6-2.png)

8. **創建Optimization Task**
    >點擊Tasks按鈕

    ![7-1](images/task_flow/optimization/7-1.png)

    >點擊Run Training Task按鈕，選擇要執行的Dataset、Model、Image、Config，及其他資訊後，按下Create按鈕

    ![8-2](images/task_flow/optimization/8-2.png)
