## 數據分析專案

- **WEKA分析研究音樂產業趨勢並連結音樂取向與心理疾病的影響**
  - 整理數據並透過簡單的敘述性分析了解數據內容
  - 利用weka中apriori及hotspot，與預測性分析則利用Bayes net,naive Bayes,logistic,及j48
  - 特殊發現:
    - 罹患心理疾病的年齡層普遍偏低
    - 常聽節奏快、震動強的音樂如遊戲音樂、搖滾樂、EDM、R&B，工作時聽音樂以及每日聆聽音樂時間過長，更有機會成為心理疾病潛在患者
    - 有強迫症的受眾對於EDM有更多的偏好(相關性強)
    - KPOP、Jazz、country跟latin對於心理疾病緩解較有效，並與關聯性分析的分群結果相同
    - Metal和Videogame Music的受眾似乎對於音樂治療的效果產生了負面影響，同與分群結果相同
  - [查看 PDF 文件](https://github.com/yichiaaa/Data_Analysis/blob/01f3fdfedebee07c2c329a05fbf7d453556ec89a/WEKA%E5%88%86%E6%9E%90%E7%A0%94%E7%A9%B6%E9%9F%B3%E6%A8%82%E7%94%A2%E6%A5%AD%E8%B6%A8%E5%8B%A2%E4%B8%A6%E9%80%A3%E7%B5%90%E9%9F%B3%E6%A8%82%E5%8F%96%E5%90%91%E8%88%87%E5%BF%83%E7%90%86%E7%96%BE%E7%97%85%E7%9A%84%E5%BD%B1%E9%9F%BF.pdf)
  
- **WEKA分析大眾對於載具使用之相容性與發票捐贈之影響**
  - 台灣每年發票量約 80 餘億張，相當於砍伐 8 萬棵樹。每本發票估算其成本約為 15 元，若完成電子化每年可節省 1,200 億元花費。
  - 利用weka中apriori及hotspot，與預測性分析則利用RandomForest,logistic,及j48
  - 特殊發現:
    - 北部地區使、具有大學以上學歷、經濟收入低於$26,400，有94.23%的載具使用者
    - 高收入且年齡較高的族群使用率較少，可以向這個族群中的居住於北部及大學教育程度以上的市場推廣載具這項服務
    - 問卷數據中顯示不知道愛心捐贈碼的樣本佔55%，且可觀察到曾經捐贈過發票但不知道捐贈碼的樣本亦是多數
    - 依變數使用"是否為載具使用者"來對此模型做RandomForest，得出結論為此模型之平均精確率達76%
  - [查看 PDF 文件](https://github.com/yichiaaa/Data_Analysis/blob/01f3fdfedebee07c2c329a05fbf7d453556ec89a/WEKA%E5%A4%A7%E7%9C%BE%E5%B0%8D%E6%96%BC%E8%BC%89%E5%85%B7%E4%BD%BF%E7%94%A8%E4%B9%8B%E7%9B%B8%E5%AE%B9%E6%80%A7%E8%88%87%E7%99%BC%E7%A5%A8%E6%8D%90%E8%B4%88%E4%B9%8B%E5%BD%B1%E9%9F%BF.pdf))

- **SQL營收與使用者資料分析**
  - 使用 XAMPP 與 phpMyAdmin 作為資料庫架設工具
  - 建置並下載了六個資料庫，並對其內部資料進行了重整與關聯性重建，並增設了 RFM 點數資料庫以提升數據洞察。
  - 使用 Python 的資料視覺化套件，如 Numpy 和 Pandas，並連接 Jupyter Notebook 來繪製視覺化圖示。
   - [SQL使用者資料分析](https://github.com/yichiaaa/Data_Analysis/blob/dbf1b8982b634ca1ba7c2217d04016effa1b7aef/sql.md)
    
- **儒鴻紡織-財務報表分析**
  - 產業分析內外部、財報報表之靜態與動態分析
  - 重要會計政策關鍵查核事項與重要會計項目分析、企業價值分析(股利折現法、現金流量折現法、市場法)
  - 特殊發現:
    - 收入認列嚴謹：產品交付時認列收入，需嚴格內控，應對外部政策影響。
    - 應收帳款管理：採用預期信用損失模型，需平衡簡化和全面理解風險。
    - 存貨管理：提前儲備原料應對供應鏈風險，以成本與淨變現價值孰低原則衡量。
    - 關係人交易透明：無異常交易，高透明度，減少盈餘管理和利益輸送風險。
  - [查看 PDF 文件](https://github.com/yichiaaa/Data_Analysis/blob/01f3fdfedebee07c2c329a05fbf7d453556ec89a/%E5%84%92%E9%B4%BB%E7%B4%A1%E7%B9%94-%E8%B2%A1%E5%8B%99%E5%A0%B1%E8%A1%A8%E5%88%86%E6%9E%90.pdf)
