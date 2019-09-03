---
layout: post
title: Google Sheet API Simple Usage
categories: [development]
tags: [ftp]
---

<br>Google Sheet 上传 csv，使用 gspread 依赖。

<br>

### `1.ss翻墙`
```python
import socks
import socket
import smtplib
def set_socket():
    socket.socket = socks.socksocket
    socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5,"127.0.0.1",1086)
    socks.wrapmodule(smtplib)
    
set_socket()
```



### `2.导入 gspread 依赖，通过认证`

```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials

scope = ['https://www.googleapis.com/auth/drive']

credentials = ServiceAccountCredentials.from_json_keyfile_name('.../API-7f3e1c913bd7.json', scope)

client = gspread.authorize(credentials)
```



### `3.根据 id 打开 spreedsheet ，需要将该 google sheet 分享给 client_email`

```python
spreadsheetId = '1SdqfOJ--wI_x9BmbpWqWFWiSglsUgN-hf1nASKGkS50'
url = 'https://docs.google.com/spreadsheets/d/' + spreadsheetId
spreadsheet = client.open_by_url(url)
```



### `4.操纵第一个 sheet`

```python
sheet1 = spreadsheet.get_worksheet(index=0)
sheet1.get_all_values()
sheet1.clear()
```



### `5.导入 csv ，默认导入第一个 sheet`

```python
client.import_csv(spreadsheetId, open('.../test weekly.csv', 'r').read())
```


<br>
<br>


