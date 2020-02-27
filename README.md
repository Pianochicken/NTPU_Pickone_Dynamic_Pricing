# NTPU_Pickone_Dynamic_Pricing
### NTPU MIS x Pickone
* ### 程式執行:
  * 方法一（Google Colab）：  
    1. Git Clone 此專案。　    
    2. 將 Data 內的 Programmation Data 資料夾上傳至雲端硬碟。 　
    3. 將 Data/Code 內的 Pickone_Dynamic_Pricing.ipynb 檔案上傳至 Google Colab。 　
    4. 開啟 ipynb 檔案即可執行。

  * 方法二（Local，Jupyter Notebook）：
    1. Git Clone 此專案。
    2. 使用 jupyter notebook 開啟 Data/Code 內的 Pickone_Dynamic_Pricing(jupyter).ipynb 檔案。
    3. 執行程式。
 
* ### 程式說明: 
   * #### 第一段： Data Processing 資料處理 
      1. spaces(mrt).csv （第二次給的，包含捷運站的spaces資料） 
      2. old_new_order_original.csv （第一次及第二次給的真實訂單合併資料） 
      3. god_account_view.csv （會員資料）  
      4. spaces_devices.csv （場地設備資訊） 
      5. space_device (Tranfer).csv （將space_device檔案內容轉置，以供後續使用） 
      6. spaces_first_time_upload.csv （第一次給予的spaces資料，取其updated_at的時間） 
      7. 產生 Real_Order.csv & Space_info.csv （產生假訂單需要的檔案） 
       
   * #### 第二段： Fake Order Generation 
      1. 讀取 Real_Order.csv & Space_info.csv 檔案。 
      2. 產生 Fake_Orders_in_Real_Orders_Time.csv （依據真實訂單的資料，產生有訂單當日但未出租時段的假訂單） 
      3. 產生 Fake_Orders_in_Empty_Time.csv （依據Space房間的資訊，產生自該房間創立到關閉的未出租時段的假訂單） 
       
   * #### 第三段： Fake & Real Orders: Combine and Filter Overlapping Parts 
      1. 將真實訂單以及假訂單做資料處理後合併。
      2. 產生 Final Fake & Real Orders.csv ，為丟進預測模型、包含真假訂單狀況的總資料檔案。
       
   * #### 第四段： Model Trianing 
      1. 讀取 Final Fake & Real Orders.csv 檔案。
      2. 訓練總體模型（Locate）及個體模型（Space）。 
      3. 比較總、個體模型的準確率，根據不同 space 選擇使用準確較高的模型，進行預訂率預測。 
     
   * #### 第五段： Price Predicting & Setting 
      1. 計算每個 space 的 leadtime，判斷預訂時間是否在 平均Leadtime 的正負一個標準差區間內。 
      2. 對照到 space 預訂率，判斷應漲價與否。
      2. 最終產生一個可對應的 Price Table。 

* ### Running Environment
   * Google Colab - Python 3.6
   * Jupyter Notebook - Python 3.6
   
* ### Predict model
   * Logistic Regression Model
   * Package: Sk-learn
   * Train/Test: 80/20
   
* ### Dataset:
   * Predict Model using Variable:
      1. orders
         * status ( 1: Fake order, 2: Real order )
      2. spaces
         * people
         * size
         * devices ( 0 ~ 19 )
      3. New created variable
         * locate_id ( 0 ~ 32 )
         * Is_holiday ( 0: Mon. ~ Fri., 1: Sat. ~ Sun. )
         * price_per_size ( price / size )
         * start_time_group ( 1: start ~ 12:00, 2: 12:00 ~ 14:00, 3: 14:00 ~ 18:00, 4: 18:00 ~ end )
         * end_time_group ( 1: start ~ 12:00, 2: 12:00 ~ 14:00, 3: 14:00 ~ 18:00, 4: 18:00 ~ end )
         * weekday ( 1: Mon. .... 7: Sun. )
         * hours ( renting hours )

