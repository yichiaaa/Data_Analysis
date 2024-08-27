# SQL營收與使用者資料分析專案

## 1. 專案簡介
本專案旨在使用 SQL 進行營收與使用者資料分析，透過數據庫的建立和管理來深入理解消費者行為及提升企業營運效益。

## 2. 使用的軟體及工具
- 使用 XAMPP 與 phpMyAdmin 作為資料庫架設的工具
<p align="center">
  <img src="https://github.com/yichiaaa/Data_Analysis/blob/f94f9b45a4f1d1b173a683b3371aaf0b99cc3304/xampp.png" alt="xampp" style="width: 40%; margin-right: 2%;">
  <img src="https://github.com/yichiaaa/Data_Analysis/blob/f94f9b45a4f1d1b173a683b3371aaf0b99cc3304/php.png" alt="phpmyadmin" style="width: 55%;">
</p>


## 3. 資料建構
<p>
  <img src="https://github.com/yichiaaa/Data_Analysis/blob/cacfe507f7ad4481a2e5dce02f38ce9daad0e4d0/menu.png" alt="menu" style="float: left; margin-right: 20px; width: 300px;"/>
  <span>
    <ul>
      <li>建置並下載了六個資料庫，並對其內部資料進行了重整和關聯性重建，總資料筆數超過一千條。</li>
      <li>資料內容涵蓋：顧客銷售時間、顧客ID、商品價格等資訊</li>
      <li>自行增設了RFM點數資料庫以提升數據洞察。</li>
      <li>資料庫還包括了關鍵字搜尋、廣告效果及流量數據等詳細資訊。</li>
    </ul>
  </span>
</p>

## 4. 資料分析

## 第一步: 了解資料

### 完成繁雜的資料處理後，首先進行簡單的敘述性資料分析。例如：
### 1. 如果希望知道訂單（order）中各類別（category）商品（product）的平均價格則

```sql
SELECT category, AVG(price) FROM orders 
INNER JOIN order_details ON orders.id = order_details.order_id 
INNER JOIN products ON order_details.product_id = products.id GROUP BY category;
```
![](https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/1.png)
​
### 2. 若希望使用 視窗函式 進行訂單（orders）金額（amount）的移動平均數計算
```
SELECT id, 
amount, 
AVG(amount) OVER (ORDER BY id ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg_price FROM orders ORDER BY created_at;
```
![](https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/2.png)

## 第二步:利用RFM分析潛在顧客
### 1. 先計算可參考資料的次數或是可參考數額
![](https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/3.png)
### 資料是從6/27~7/30的網站瀏覽及購買資料，可以發現在這段期間裡消費者最常觀望的商品為133及1343，但消費者最終都選擇1343甚至購買，後續可以了解133及1343的產品是否有關連性，或是1343較優異的地方為何。其中更可以發現被購買最多的產品為53343。

### 2. 後自行定義RFM Point 分數進行分析
```
CREATE VIEW RFM_points2
AS
SELECT
    user_id,
    recency,
    monetary,
    frequency,
    CASE
        WHEN recency < 2 THEN 5
        WHEN recency < 7 THEN 4
        WHEN recency < 15 THEN 3
        WHEN recency < 31 THEN 2
        ELSE 1
    END AS recency_point,
    CASE
        WHEN frequency > 10 THEN 5
        WHEN frequency > 5 THEN 4
        WHEN frequency > 3 THEN 3
        WHEN frequency > 2 THEN 2
        ELSE 1
    END AS frequency_point,
    CASE
        WHEN monetary > 10000 THEN 5
        WHEN monetary > 5000 THEN 4
        WHEN monetary > 2000 THEN 3
        WHEN monetary > 500 THEN 2
        ELSE 1
    END AS monetary_point
FROM RFM;
```
![](https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/4.png)
![](https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/5.png)
### 透過RFM分析得知，最有價值的使用者依序應為5、2，6與1則並列第三，這些會員將是未來需重點關注及服務的對象。

## 第三步:資料視覺化
### 利用各種資料視覺化套件如Numpy、pandas、連接jupyter notebook都可以來繪製視覺畫圖示

<img src="https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/6.png" alt="chart 1" style="display: block; margin: 0 auto; width: 70%;"/>
<div style="display: flex; justify-content: center; margin-top: 20px;">
  <img src="https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/7.png" alt="chart 2" style="width: 45%; margin-right: 10px;"/>
  <img src="https://github.com/yichiaaa/Data_Analysis/blob/dc7d597727fa938a0c1374de065dd93de16c335e/8.png" alt="chart 3" style="width: 45%;"/>
</div>


### 範例程式碼如下

```
import pandas as pd
import matplotlib.pyplot as plt
# 引入 Python 內建網址解析套件
from urllib.parse import urlsplit, parse_qs
# 引用 pymsql 套件
import pymysql

connection = pymysql.connect(
   host='localhost',
   user='demo_user',
   password='0123',
   # 資料庫預設為 3306 若自己有更改不同 port 請依照需求更改
   port=3306,
   db='demo_shop_logs',
   charset='utf8mb4',
   cursorclass=pymysql.cursors.DictCursor)

# 嘗試連線並執行 SQL 查詢
try:
    with connection.cursor() as cursor:
        sql = 'SELECT * FROM user_purchase_logs'
        cursor.execute(sql)
        items = cursor.fetchall()
finally:
    connection.close()

# 建立空字典以儲存UTM統計資料
utm_stats = {'utm_source': {}, 'utm_medium': {}, 'utm_campaign': {}}

# 分析資料並填入字典
for item in items:
    referrer = item['referrer']
    query_str = urlsplit(referrer).query
    query_dict = parse_qs(query_str)
    
    for query_key, query_value in query_dict.items():
        utm_value = query_value[0]
        if utm_value in utm_stats[query_key]:
            utm_stats[query_key][utm_value] += 1
        else:
            utm_stats[query_key][utm_value] = 1

# 將UTM統計資料轉換為pandas Series
df_utm_source = pd.Series(utm_stats['utm_source'])
df_utm_medium = pd.Series(utm_stats['utm_medium'])
df_utm_campaign = pd.Series(utm_stats['utm_campaign'])

# 繪製圓餅圖
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

# UTM Source
axes[0].pie(df_utm_source, labels=df_utm_source.index, autopct='%1.1f%%', startangle=140)
axes[0].set_title('UTM Source Distribution')
axes[0].axis('equal')  # 使圓餅圖比例相等

# UTM Medium
axes[1].pie(df_utm_medium, labels=df_utm_medium.index, autopct='%1.1f%%', startangle=140)
axes[1].set_title('UTM Medium Distribution')
axes[1].axis('equal')

# UTM Campaign
axes[2].pie(df_utm_campaign, labels=df_utm_campaign.index, autopct='%1.1f%%', startangle=140)
axes[2].set_title('UTM Campaign Distribution')
axes[2].axis('equal')

plt.tight_layout()
plt.show()
```
