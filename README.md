# NTPU_Pickone_Dynamic_Pricing
### NTPU MIS x Pickone

* ### Running Environment
   * Google Colab - Python 3.6
* ### Predict model
   * Logistic Regression Model
   * Package: Sk-learn
   * Train/Validation/Test: 60/20/20
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

* ### ER model:

![ER model](https://github.com/Pianochicken/NTPU_Pickone_Dynamic_Pricing/blob/master/Data/ERD_all.jpg "ER model-all")
