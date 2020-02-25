# NTPU_Pickone_Dynamic_Pricing
### NTPU MIS x Pickone
* ### 程式執行:
  * 方法一（Google Colab）：  
    1. Git Clone 此專案。　    
    2. 將 Data 內的 Programmation Data 資料夾上傳至雲端硬碟。 　
    3. 將 Data/Code 內的 Pickone_Dynamic_Pricing.ipynb 檔案上傳至 Google Colab。 　
    4. 開啟 ipynb 檔案即可執行。

  * 方法二（Local，需修改讀取檔案路徑）：
    1. Git Clone 此專案。
    2. 使用 jupyter notebook 開啟 Data/Code 內的 Pickone_Dynamic_Pricing.ipynb 檔案。
    3. 註解前三個cell，修改檔案的讀取路徑。
    4. 執行程式。

* ### 程式說明:
    第一段： Data Processing 資料處理
      1. spaces(mrt).csv (第二次給的，包含捷運站的spaces資料)
      2. old_new_order_original.csv (第一次及第二次給的真實訂單合併資料)
      3. god_account_view.csv (會員資料) 
      4. spaces_devices.csv (場地設備資訊)
      5. 產生 space_device (Tranfer).csv (將space_device檔案內容轉置，以供後續使用)
      6. spaces_first_time_upload.csv (第一次給予的spaces資料，取其updated_at的時間)
      7. 最終產生出 Real_Order.csv & Space_info.csv (提供產生假訂單的使用)
      
    第二段： Fake Order Generation
      1. 讀取 Real_Order.csv & Space_info.csv 檔案。
      2. 產生Fake_Orders_in_Real_Orders_Time.csv (依據真實訂單的資料，產生有訂單當日但未出租時段的假訂單)
      3. 產生Fake_Orders_in_Empty_Time.csv (依據Space房間的資訊，產生自該房間創立到關閉的未出租時段的假訂單)
      
    第三段： Fake & Real Orders: Combine and Filter Overlapping Parts
      1. 將真實訂單以及假訂單做資料處理後合併，產生一個可丟進預測模型、包含真假訂單狀況的總資料檔案。
      
    第四段： Model Trianing
      1. 讀取 Fake_Orders_in_Real_Orders_Time.csv 檔案，訓練總體模型（Locate）及個體模型（Space）。
    
    第五段： Price Predicting & Setting
      1. 計算每個 Space 的 Leadtime，判斷預訂時間是否在平均 Leadtime 的正負一個標準差區間內。
      2. 若是，使用預測模型預測預訂率，根據預訂率決定漲價與否。若否，則維持原價
      3. 產生一個可對應的Price Table

* ### Running Environment
   * Google Colab - Python 3.6
* ### Predict model
   * Logistic Regression Model
   * Package: Sk-learn
   * Train/Test: 80/20
* ### Dataset:
   1. Total Database using Variable: All related data.
   2. Predict Model using Variable:
      1. orders
         * space_id
         * category
      2. spaces
         * space_id
         * people
         * size
         * order_price
         * price
         * zipcode
      3. New created variable
         * is_holiday ( 0: Mon. ~ Fri., 1: Sat. ~ Sun. )
         * per_person_price ( price / people )
         * time_group ( 1: start ~ 12:00, 2: 12:00 ~ 14:00, 3: 14:00 ~ 18:00, 4: 18:00 ~ end )
         * weekday ( 1: Mon. .... 7: Sun. )
         * hours ( renting hours )

