# NTPU_Pickone_Dynamic_Pricing
### NTPU MIS x Pickone
* ### 程式執行：
  * #### 方法一（Google Colab）：
    1. Git Clone 此專案。　    
    2. 將 Data 內的 Programmation Data 資料夾上傳至雲端硬碟。 　
    3. 將 Data/Code 內的 Pickone_Dynamic_Pricing.ipynb 檔案上傳至 Google Colab。 　
    4. 在 Google Colab 開啟檔案並執行。（在上方 Runtime 的 Change runtime type 可改用 GPU 執行，速度較快）

  * #### 方法二（Local，Jupyter Notebook）：
    1. Git Clone 此專案。
    2. 使用 jupyter notebook 開啟 Data/Code 內的 Pickone_Dynamic_Pricing(jupyter).ipynb 檔案。
    3. 執行程式。

* ### 程式會使用到的資料表：
   * #### 原始資料表：
      1. spaces.csv (檔案需替換成最新的spaces.csv)
      2. orders.csv (檔案需替換成最新的orders.csv)
      3. god_account_view.csv
      4. spaces_devices.csv
      5. spaces_first_time_upload.csv（第一次給予的spaces.csv，取其updated_at的時間，此檔案需保留。）
   
   * #### 程式內產生的資料表：
      1. space_device (Tranfer).csv （將space_device檔案內容轉置，以供後續使用） 
      2. Real_Order.csv 以及 Space_info.csv （產生假訂單需要的檔案）
      3. Fake_Orders_in_Real_Orders_Time.csv （依據真實訂單的資料，產生有訂單當日但未出租時段的假訂單） 
      4. Fake_Orders_in_Empty_Time.csv （依據Space房間的資訊，產生自創立到關閉的未出租時段的假訂單） 
      5. Final Fake & Real Orders.csv （丟進預測模型、包含真假訂單狀況的總資料檔案）
      6. final_table.csv（最後客戶下訂所參照的表格，決定是否調整價格）


* ### 程式簡介（共分為五個部分）：
   * #### 第一段： Data Processing 資料處理  
      * 將資料依據 Working Log/Working Process Record.docx 進行處理。
      * 產生 Real_Order.csv & Space_info.csv 
       
   * #### 第二段： Fake Order Generation 產生假訂單 
      * 產生 Fake_Orders_in_Real_Orders_Time.csv 
      * 產生 Fake_Orders_in_Empty_Time.csv
       
   * #### 第三段： Fake & Real Orders: Combine and Filter Overlapping Parts 真假訂單合併
      * 產生 Final Fake & Real Orders.csv
       
   * #### 第四段： Model Trianing 訓練模型
      * 訓練總體模型（Locate）及個體模型（Space）。 
      * 比較總、個體模型的準確率，根據不同 space 選擇使用準確較高的模型，進行預訂率預測。 
      * 計算每個 space 在各個時段的出租預測機率。
     
   * #### 第五段： Price Predicting & Setting 
      * 計算每個 space 的 平均leadtime 以及 標準差 Standard deviation。 
      * 最終產生一個可供參考對應的 final_table.csv。
      * 對照表格的 space 預訂率，作為漲價與否的依據。


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

