# Summary
* [簡介]
  *  [AI/ML Intelligent Platform介紹]
  *  [Deployment Platform介紹]
  *  [Edge Server介紹]
  *  [抽象類別階層](./inference_system/abstract_class_hierarchy.md)
  *  [開發人員]
* [平台架構圖]
  * [整體架構圖](./architecture/architecture.md#平台整體架構圖)
  * [AI/ML Intelligent Plafrom](./architecture/architecture.md#aiml-intelligent-platform)
  * [Deployment Platform](./architecture/architecture.md#部屬平台deployment-platform)
  * [Edge Server](./architecture/architecture.md#邊緣伺服器edge-server)
* [部屬]
  * [Deployment Platform](./deployment/deployment_platform/deployment.md)
  * [Edge Server](./deployment/edge_server/deployment.md)
* [使用手冊]
  * [整體使用流程](./user_manual/task_flow.md#整體使用流程)
    * [如何執行一個Preprocessing Task](./user_manual/task_flow.md#如何執行一個-preprocessing-task)
    * [如何執行一個Training Task](./user_manual/task_flow.md#如何執行一個training-task)
    * [如何執行一個Optimization Task](./user_manual/task_flow.md#如何執行一個optimization-task)
    * [如何執行一個Evaluation Task](./user_manual/task_flow.md#如何執行一個evaluation-task)
  * [AI/ML Intelligent Platfrom]
    * [API操作說明]
      * [API SERVER](./user_manual/api_doc/api_server.md)
      * Dataset管理
        * [上傳訓練用的Original/Training Dataset檔案](./user_manual/api_doc/dataset.md#上傳訓練用的originaltraining-dataset檔案)
        * [上傳優化用的Original/Optimization Dataset檔案](./user_manual/api_doc/dataset.md#上傳優化用的originaloptimization-dataset檔案)
        * [查詢Project底下訓練用的Original/Training Dataset資訊](./user_manual/api_doc/dataset.md#查詢project底下訓練用的orignaltraining-dataset資訊)
        * [查詢Application底下優化用的Original/Optimization Dataset資訊](./user_manual/api_doc/dataset.md#查詢application底下優化用的originaloptimization-dataset資訊)
      * Pipeline管理
        * [上傳Preprocessing/Training/Optimization Pipeline檔案](./user_manual/api_doc/pipeline.md#上傳preprocessingtrainingoptimization-pipeline檔案)
        * [查詢Application底下的Preprocessing/Training/Optimization Pipeline資訊](./user_manual/api_doc/pipeline.md#查詢application底下的preprocessingtrainingoptimization-pipeline資訊)
      * Build File管理
        * [上傳Preprocessing/Training/Optimization Build File檔案](./user_manual/api_doc/build_file.md#上傳preprocessingtrainingoptimization-build-file檔案)
        * [查詢Pipeline底下的Preprocessing/Training/Optimization Build File資訊](./user_manual/api_doc/build_file.md#查詢pipeline底下的preprocessingtrainingoptimization-build-file資訊)
      * Config管理
        * [新增Preprocessing/Training/Optimization Config資訊](./user_manual/api_doc/config.md#新增preprocessingtrainingoptimization-config資訊)
        * [查詢Pipeline底下的Preprocessing/Training/Optimization Config資訊](./user_manual/api_doc/config.md#查詢pipeline底下的preprocessingtrainingoptimization-config資訊)
      * Pipeline運行
        * [運行Preprocessing Task](./user_manual/api_doc/run_pipeline.md#運行preprocessing-task)
        * [運行Training Task](./user_manual/api_doc/run_pipeline.md#運行training-task)
        * [運行Optimization Task](./user_manual/api_doc/run_pipeline.md#運行optimization-task)
        * [查看單一Preprocessing/Training/Optimization Task資訊](./user_manual/api_doc/run_pipeline.md#查看單一preprocessingtrainingoptimization-task資訊)
        * [查看Preprocessing/Training/Optimization Task Log](./user_manual/api_doc/run_pipeline.md#查看preprocessingtrainingoptimization-task-log)
      * Model管理
        * [下載Model檔案](./user_manual/api_doc/model.md#下載model檔案)
        * [查看所屬Application的Model資訊](./user_manual/api_doc/model.md#查看所屬application的model資訊)
    * [Pipeline範例]
    * [Image範例]
    * [Template範例](./user_manual/inference.md)
    * [SDK範例]
    * [前端頁面]
      * Project
        * [新增Project](./user_manual/project.md#新增project)
        * [更新Project](./user_manual/project.md#更新project)
        * [刪除Project](./user_manual/project.md#刪除projct)
      * Application
        * [查看所有Application](./user_manual/application.md#查看所有application)
        * [新增Application](./user_manual/application.md#新增application)
        * [更新Application](./user_manual/application.md#更新application)
        * [刪除Application](./user_manual/application.md#刪除application)
      * Project Dataset
        * [查看所有Project Dataset](./user_manual/project_dataset.md#查看所有project-dataset)
        * [新增Project Dataset](./user_manual/project_dataset.md#新增project-dataset)
        * [更新Project Dataset](./user_manual/project_dataset.md#更新originaltraining-dataset)
        * [下載Project Dataset](./user_manual/project_dataset.md#下載originaltraining-dataset)
        * [搜尋Project Dataset](./user_manual/project_dataset.md#搜尋originaltraining-dataset)
        * [刪除Project Dataset](./user_manual/project_dataset.md#刪除originaltraining-dataset)
      * Model
        * [查看所有Model](./user_manual/model.md#查看所有model)
        * [新增Model](./user_manual/model.md#新增model)
        * [更新Model](./user_manual/model.md#更新model)
        * [下載Model](./user_manual/model.md#下載model)
        * [發佈/取消發佈Model](./user_manual/model.md#發佈取消發佈model)
        * [刪除Model](./user_manual/model.md#刪除model)
      * Preprocessing Pipeline
        * [查看所有Preprocessing Pipeline](./user_manual/preprocessing/pipeline.md#查看所有preprocessing-pipeline)
        * [新增Preprocessing Pipeline](./user_manual/preprocessing/pipeline.md#新增preprocessing-pipeline)
        * [更新Preprocessing Pipeline](./user_manual/preprocessing/pipeline.md#更新preprocessing-pipeline)
        * [下載Preprocessing Pipeline](./user_manual/preprocessing/pipeline.md#下載preprocessing-pipeline)
        * [刪除Preprocessing Pipeline](./user_manual/preprocessing/pipeline.md#刪除preprocessing-pipeline)
        * Preprocesing Config
          * [查看所有Preprocessing Config](./user_manual/preprocessing/config.md#查看所有preprocessing-config)
          * [新增Preprocessing Config](./user_manual/preprocessing/config.md#新增preprocessing-config)
          * [更新Preprocessing Config](./user_manual/preprocessing/config.md#更新preprocessing-config)
          * [刪除Preprocessing Config](./user_manual/preprocessing/config.md#刪除preprocessing-config)
        * Preprocessing Image
          * [查看所有Preprocessing Image](./user_manual/preprocessing/image.md#查看所有preprocessing-image)
          * [新增Preprocessing Image](./user_manual/preprocessing/image.md#新增preprocessing-image)
          * [更新Preprocessing Image](./user_manual/preprocessing/image.md#更新preprocessing-image)
          * [下載Preprocessing Image](./user_manual/preprocessing/image.md#下載preprocessing-image)
          * [刪除Preprocessing Image](./user_manual/preprocessing/image.md#刪除preprocessing-image)
        * Preprocessing Task
          * [查看所有Preprocessing Task](./user_manual/preprocessing/task.md#查看所有preprocessing-task)
          * [新增Preprocessing Task](./user_manual/preprocessing/task.md#新增preprocessing-task)
          * [更新Preprocessing Task](./user_manual/preprocessing/task.md#更新preprocessing-task)
          * [下載Preprocessing Task Log](./user_manual/preprocessing/task.md#下載preprocessing-task-log)
          * [刪除Preprocessing Task](./user_manual/preprocessing/task.md#刪除preprocessing-task)
      * Optimization Dataset
        * [查看所有Optimization Dataset](./user_manual/application_dataset.md#查看所有optimization-dataset)
        * [新增Optimization Dataset](./user_manual/application_dataset.md#新增optimization-dataset)
        * [更新Optimization Dataset](./user_manual/application_dataset.md#更新originaloptimization-dataset)
        * [下載Optimization Dataset](./user_manual/application_dataset.md#下載originaloptimization-dataset)
        * [搜尋Optimization Dataset](./user_manual/application_dataset.md#搜尋originaloptimization-dataset)
        * [刪除Optimization Dataset](./user_manual/application_dataset.md#刪除originaloptimization-dataset)
      * Training Pipeline
        * [查看所有Training Pipeline](./user_manual/training/pipeline.md#查看所有training-pipeline)
        * [新增Training Pipeline](./user_manual/training/pipeline.md#新增training-pipeline)
        * [更新Training Pipeline](./user_manual/training/pipeline.md#更新training-pipeline)
        * [下載Training Pipeline](./user_manual/training/pipeline.md#下載training-pipeline)
        * [刪除Training Pipeline](./user_manual/training/pipeline.md#刪除training-pipeline)
        * Training Config
          * [查看所有Training Config](./user_manual/training/config.md#查看所有training-config)
          * [新增Training Config](./user_manual/training/config.md#新增training-config)
          * [更新Training Config](./user_manual/training/config.md#更新training-config)
          * [刪除Training Config](./user_manual/training/config.md#刪除training-config)
        * Training Image
          * [查看所有Training Image](./user_manual/training/image.md#查看所有training-image)
          * [新增Training Image](./user_manual/training/image.md#新增training-image)
          * [更新Training Image](./user_manual/training/image.md#更新training-image)
          * [下載Training Image](./user_manual/training/image.md#下載training-image)
          * [刪除Training Image](./user_manual/training/image.md#刪除training-image)
        * Training Task
          * [查看所有Training Task](./user_manual/training/task.md#查看所有training-task)
          * [新增Training Task](./user_manual/training/task.md#新增training-task)
          * [更新Training Task](./user_manual/training/task.md#更新training-task)
          * [下載Training Task Log](./user_manual/training/task.md#下載training-task-log)
          * [刪除Training Task](./user_manual/training/task.md#刪除training-task)
      * Optimization Pipeline
        * [查看所有Optimization Pipeline](./user_manual/optimization/pipeline.md#查看所有optimization-pipeline)
        * [新增Optimization Pipeline](./user_manual/optimization/pipeline.md#新增optimization-pipeline)
        * [更新Optimization Pipeline](./user_manual/optimization/pipeline.md#更新optimization-pipeline)
        * [下載Optimization Pipeline](./user_manual/optimization/pipeline.md#下載optimization-pipeline)
        * [刪除Optimization Pipeline](./user_manual/optimization/pipeline.md#刪除optimization-pipeline)
        * Optimization Config
          * [查看所有Optimization Config](./user_manual/optimization/config.md#查看所有optimization-config)
          * [新增Optimization Config](./user_manual/optimization/config.md#新增optimization-config)
          * [更新Optimization Config](./user_manual/optimization/config.md#更新optimization-config)
          * [刪除Optimization Config](./user_manual/optimization/config.md#刪除optimization-config)
        * Optimization Image
          * [查看所有Optimization Image](./user_manual/optimization/image.md#查看所有optimization-image)
          * [新增Optimization Image](./user_manual/optimization/image.md#新增optimization-image)
          * [更新Optimization Image](./user_manual/optimization/image.md#更新optimization-image)
          * [下載Optimization Image](./user_manual/optimization/image.md#下載optimization-image)
          * [刪除Optimization Image](./user_manual/optimization/image.md#刪除optimization-image)
        * Optimization Task
          * [查看所有Optimization Task](./user_manual/optimization/task.md#查看所有optimization-task)
          * [新增Optimization Task](./user_manual/optimization/task.md#新增optimization-task)
          * [更新Optimization Task](./user_manual/optimization/task.md#更新optimization-task)
          * [下載Optimization Task Log](./user_manual/optimization/task.md#下載optimization-task-log)
          * [刪除Optimization Task](./user_manual/optimization/task.md#刪除optimization-task)
      * Evaluation Pipeline
        * [查看所有Evaluation Pipeline](./user_manual/evaluation/pipeline.md#查看所有evaluation-pipeline)
        * [新增Evaluation Pipeline](./user_manual/evaluation/pipeline.md#新增evaluation-pipeline)
        * [更新Evaluation Pipeline](./user_manual/evaluation/pipeline.md#更新evaluation-pipeline)
        * [下載Evaluation Pipeline](./user_manual/evaluation/pipeline.md#下載evaluation-pipeline)
        * [刪除Evaluation Pipeline](./user_manual/evaluation/pipeline.md#刪除evaluation-pipeline)
        * Evaluation Config
          * [查看所有Evaluation Config](./user_manual/evaluation/config.md#查看所有evaluation-config)
          * [新增Evaluation Config](./user_manual/evaluation/config.md#新增evaluation-config)
          * [更新Evaluation Config](./user_manual/evaluation/config.md#更新evaluation-config)
          * [刪除Evaluation Config](./user_manual/evaluation/config.md#刪除evaluation-config)
        * Evaluation Image
          * [查看所有Evaluation Image](./user_manual/evaluation/image.md#查看所有evaluation-image)
          * [新增Evaluation Image](./user_manual/evaluation/image.md#新增evaluation-image)
          * [更新Evaluation Image](./user_manual/evaluation/image.md#更新evaluation-image)
          * [下載Evaluation Image](./user_manual/evaluation/image.md#下載evaluation-image)
          * [刪除Evaluation Image](./user_manual/evaluation/image.md#刪除evaluation-image)
        * Evaluation Task
          * [查看所有Evaluation Task](./user_manual/evaluation/task.md#查看所有evaluatiion-task)
          * [新增Evaluation Task](./user_manual/evaluation/task.md#新增evaluatiion-task)
          * [更新Evaluation Task](./user_manual/evaluation/task.md#更新evaluatiion-task)
          * [下載Evaluation Task Log](./user_manual/evaluation/task.md#下載evaluatiion-task-log)
          * [刪除Evaluation Task](./user_manual/evaluation/task.md#刪除optimization-task)
  * [Deployment Platform]
  * [Edge Server]
    * [關於 xApp/rApp](./xApp_rApp/about_xApp_rApp.md)
    * [建置 xApp/rApp](./xApp_rApp/build_xApp_rApp.md)
* [常見問題](./issue/README.md)