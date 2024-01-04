---
title: "HTTP協議"
date: 2024-01-04T11:30:45+08:00
tags:
categories:
draft: false

---
# HTTP 協議

###### tags: `學習心得` `HTTP`

> 細節大多以最廣泛的HTTP 1.1 討論

## HTTP 是什麼?

開啟瀏覽器，在搜尋欄上打上資料送出，過了幾秒，瀏覽器也很聽話地把我們搜尋的玩意顯示在畫面。為甚麼這麼神奇?HTTP在裡面扮演什麼腳色?

HTTP 就像是瀏覽器和伺服器的法律。由客戶端發送請求，等待接收回應。

## **Http 特點**

![http_header_struct.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/659ba55a-3e97-438b-bc83-c57e4564cacd/http_header_struct.jpg)

- HTTP Header 是可選的，可以包含任意數據
- HTTP Header 放置請求方法，檔案類型等等。
- HTTP Body 常見參數的放置區域(post , put)，也就是html header 、html body

---

## Http Header 種類(四大)

[Heder 詳細介紹附錄](https://www.notion.so/Heder-fa0950dd27814476aa25657e03e8d962?pvs=21)

1.  General (不屬於headers , 收集 request url ,response ,status等等)

    
    https://segmentfault.com/img/bVboBwP
    
2. Request Headers 客戶端傳送過來的請求

    
    https://segmentfault.com/img/bVboBxd
    
3. Response Header  主機端回應的請求

    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/93a158b7-b617-491c-b0ba-8feac95278b1/Untitled.png)
    
4. **Entity Header 實體主機的更多信息(例如 content-length, mime type**

    
    **Allow——可接受的檔案類型，以MIME TYPE呈現
    Content-Encoding——壓縮格式
    Content-Language——語言
    Content-Length——檔案大小
    Content-Type——實際返回類型，以MIME TYPE呈現
    Expires——包含日期/時間， 即在此時候之後，響應過期
    Last-Modified——資源的最後修改日期時間**
    

---

## 具體應用

1.  快取機制
 參考 cache-control 或者 expires 是否有效

    
    ```bash
    Response Headers
    	accept-ranges: bytes
    	age: 49382748
    	**cache-control: max-age=315360000
    	Last-Modified: Fri, 04 Mar 2022 09:39:45 GMT
    	Etag "cc14a-3ab-48e27e3975c0**
    ```
    
    > max-age 為緩存的毫秒數
    > 
    
    判斷過期的方法
    **Etag 標籤是否一致 , Last-Modified 上次文件修改的時間**
    
    如果過期的話，瀏覽器會向主機端要求一份新的檔案。
    
    如果不希望瀏覽器有快取可以這麼設定
    
    ```bash
    Cache-Control: max-age=0, no-cache
    ```
    

1.  MIME TYPE
瀏覽器會通過mime type 來決定如何處理檔案，所以設置正確的mime type是非常重要的

2. 跨域問題
瀏覽器為了避免XSS, CSFR等攻擊，設立了同源政策(協議+域名+端口)三者相同，才不會產生跨域問題。

但是開發過程中，總是會遇到需要跨域的時機，例如不同port，這時就要去設定允許跨域請求的header 參數。

跨域有分成兩種

1.  簡單請求(不會觸發CROS) 不用設定
    
    > 需要滿足下列**所有**條件：
    > 
    > 
    > 第一條: 請求方式必須為GET | HEAD | POST
    > 
    > 第二條: Content-Type 的值必須屬於下列之一:application/x-www-form-urlencoded | multipart/form-data | text/plain
    > 
    > 第三條: 不會發送自定義header（如X-Modified）
    > 
    
    1. 複雜請求，簡單請求的反之
    
    在發送過去的request 的header 添加以下的參數
    
    ```bash
    Access -Control-Allow-Origin: *
    Access -Control-Allow-Methods: POST, GET , OPTIONS 
    Access -Control-Allow-Headers: X-PINGOTHER, Content- Type 
    Access -Control-Max-Age: 86400
    ```
    
    **Access-Control-Allow-Origin**
    
    (1)* (允許全部)
    
    (2)具體的域名<origin>
    
    **Access -Control-Allow-Methods**
    
    跨域允許的方法
    
    ```bash
    Access-Control-Allow-Methods: PUT, POST, GET, DELETE, OPTIONS
    ```
    
    **Access -Control-Allow-Headers**
    如果需要基礎標頭(Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma)以外的資訊就需要設置
    
    **Access-Control-Max-Age**
    指定了預檢請求的結果能夠被緩存多久，在此時間內，不會再發起預檢請求。
    
    ```bash
    **Access -Control- Max -Age: <seconds>**
    ```
    

## 八種動作

> HTTP/1.1 一共定義了九種動作來操作指定的資源
> 

### GET

**請求可被快取**

使用GET方法時，會將參數附加在URL作為QueryString(key/value的組合)的一部分

例如:

[http://tw.search.com/search?](http://tw.search.com/search?p)**param=value1&param2=value2&param3=value3**
從網址就可以很明顯地觀察出參數，因此GET通常不會作為較為私密的請求

### POST、PUT、Patch

三個都很像，POST最常用。

參數可以使用queryString 和 body。

### Delete

顧名思義，刪除。

參數可以使用queryString

### Head

和GET很像，專注獲取HTTP Header資料的功能

### Option

非常少用，驗證url是否正常，正常會回傳方法(GET，POST...等)

### Connect

因應SSL誕生的方法，[詳細](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Methods/CONNECT)

### Trace

debug 方法，可以返回客戶端任意資料，可用來XSS、XST攻擊。
**非必要請禁用**
