# 政府公開資料 API 爬蟲專案

## 1. 專案簡介

本專案旨在使用 Python 中的 `requests` 和 `csv` 套件從政府公開資料網站抓取氣象觀測資料，並將其儲存為 CSV 格式。目標是從 [自動氣象站-氣象觀測資料](https://data.gov.tw/dataset/9176) API 獲取數據並將其寫入名為 `weather.csv` 的檔案。

## 2. 使用的工具

- `requests`：用於發送 HTTP 請求並獲取 API 回應。
- `csv`：用於將獲取的資料寫入 CSV 檔案。
- `apscheduler`：用於實現定時執行的功能。

## 3. 程式碼範例

以下是完整的 Python 程式碼，用於從 API 抓取資料並將其存入 CSV 檔案：

### 單次資料抓取

```python
import requests
import csv

headers = {
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.105 Safari/537.36'
}

API_URL = 'https://opendata.cwa.gov.tw/api/v1/rest/datastore/O-A0001-001?Authorization=rdec-key-123-45678-011121314'

# 發出網路請求
resp = requests.get(API_URL, headers=headers)

# 使用 json 方法將回傳值從 JSON 格式轉成 Python dict 格式方便存取
data = resp.json()
location_records = data['records']['Station']
row_list = []

# 一一取出資料
for location_record in location_records:
    lat = location_record['GeoInfo']['Coordinates'][0]['CoordinateFormat']
    lon = location_record['GeoInfo']['Coordinates'][0]['CoordinateName']
    location_name = location_record['StationName']
    station_id = location_record['StationId']
    obs_time = location_record['ObsTime']['DateTime']
    ele = location_record['GeoInfo']['StationAltitude'] 
    wdir = location_record['WeatherElement']['WindDirection']
    wdsd = location_record['WeatherElement']['WindSpeed']
    temp = location_record['WeatherElement']['AirTemperature']
    humd = location_record['WeatherElement']['RelativeHumidity']
    pres = location_record['WeatherElement']['AirPressure']
    
    # 將資料整理成 dict
    data = {
        'lat': lat,
        'lon': lon,
        'locationName': location_name,
        'stationId': station_id,
        'obsTime': obs_time,
        'ELE': ele,
        'WDIR': wdir,
        'WDSD': wdsd,
        'TEMP': temp,
        'HUMD': humd,
        'PRES': pres
    }

    # 加入到 row_list 中
    row_list.append(data)

# 印出資料內容
print(row_list)

# 設定 CSV 檔案標題
headers = ['lat', 'lon', 'locationName', 'stationId', 'obsTime', 'ELE', 'WDIR', 'WDSD', 'TEMP', 'HUMD', 'PRES']

# 將資料寫入 CSV 檔案
with open('weather_data.csv', 'w') as output_file:
    dict_writer = csv.DictWriter(output_file, headers)
    dict_writer.writeheader()  # 寫入標題
    dict_writer.writerows(row_list)  # 寫入資料
```



