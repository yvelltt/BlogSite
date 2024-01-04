---
title: "Hugo + Github + Netify 簡易靜態網站架設"
date: 2024-01-04T11:44:59+08:00
tags:
categories:
draft: false

---

### 1. 安裝Hugo

[參考連結](https://rdnotes.com/hugo-basic-get-started/)

windows 安裝環境介紹

* [hugo realse](https://github.com/gohugoio/hugo/releases)
    選擇安裝 hugo_extended_(版本號碼)_windows-amd64
* 設定環境變數
    1. 控制台\系統及安全性\系統，選擇進階系統設定
    ![static](/images/hugo_netify/hugo_netify_enviroment_01.png)

    2. 選擇系統變數，點選Path
    ![static](/images/hugo_netify/hugo_netify_enviroment_02.png)

    3. 新增hugo.exe 存放的路徑
    ![static](/images/hugo_netify/hugo_netify_enviroment_03.png)

* 安裝好hugo，變可以使用new一個Project
    ``` shell
    hugo new site 網站的名稱
    ```

* 安裝主題

    [主題列表](https://themes.gohugo.io/)


    1. 假設選擇這個[主題](https://github.com/adityatelange/hugo-PaperMod)


    1. 在Project 底下開啟CMD，輸入以下指令
    ``` shell
    git submodule add https://github.com/nodejh/hugo-theme-mini.git themes/mini
    ```

* set 環境變數
    在專案底下有config.toml檔案去設定主題樣式，以及其他參數
    ``` ini
    # 使用的主題
    theme = "mini"

    # 網站網址，可先維持預設(等上線到netlify在更改成netlify的網址，或是自定義的網域名稱)
    baseURL = 'https://wystuff.netlify.app/'

    # 預設網站語言
    languageCode = 'zh-TW'

    # 網站標題
    title = 'wyStuff'

    # 部署時執行的指令
    [build]
    publish = "public"
    command = "hugo --gc --minify"

    # 指定Hugo版本，應與本機安裝的Hugo版本一致
    [build.environment]
    HUGO_VERSION = "0.121.1"
    ```

### 2. 在本地測試

在專案底下開啟CMD，輸入以下指令
``` shell
hugo serve
```
會出現以下的shell 的狀況
``` shell
Watching for changes in C:\Users\chelsea\mywebsite\{archetypes,assets,content,data,i18n,layouts,static,themes}
Watching for config changes in C:\Users\chelsea\mywebsite\config.toml
Start building sites …
hugo v0.121.1-00b46fed8e47f7bb0a85d7cfc2d9f1356379b740+extended windows/amd64 BuildDate=2023-12-08T08:47:45Z VendorInfo=gohugoio


                   | EN
-------------------+-----
  Pages            | 15
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  5
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Built in 75 ms
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Web Server is available at http://localhost:1313/ (bind address 127.0.0.1) 這一段的http://localhost:1313/網址開起來即可查看本地狀況

### 3. 連接Github

把所有檔案推到github 上的repositories

1. 先建立repository

![static](/images/hugo_netify/hugo_netify_github_01.png)

![static](/images/hugo_netify/hugo_netify_github_02.png)


2. 輸入以下的指令
``` shell
git init
git add -A 
git commit -m "init"
git remote add origin "your github project url"
git push -u origin master
```


### 上傳到Netlify

* 01 新建一個Site
![static](/images/hugo_netify/hugo_netify_netifySet_01.png)

* 02 選取第一個Github 發布
![static](/images/hugo_netify/hugo_netify_netifySet_02.png)

* 03 選取要連結的專案
![static](/images/hugo_netify/hugo_netify_netifySet_03.png)

* 04 設置Build comman 和 Publish Directory 通常預設會自動判斷好
![static](/images/hugo_netify/hugo_netify_netifySet_04.png)



### 平常發布

1. 使用vs code 開啟專案
2. 下command 新增 md 檔案

``` shell
hugo new content posts/要新的的檔案名稱 +  副檔名(.md)
```
3. 推上github 便會自動部屬(因為已經連接netify)

4. 上傳的圖片必須放在Static 的images路徑底下，可以在底下建立多層資料夾，連結寫法從/images/yourimage.jpg
