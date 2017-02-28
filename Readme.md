# 使用 Jekyll 搭建的 github Pages
> 具體搭建過程參見： http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html

github Pages 可以理解成可編寫並能託管在 github 上的靜態網頁。可以使用 github 的模板，在站內生存網頁。也可以自己編輯網頁再上傳，但是這種法式需要經過 Jekyll 的處理。 

## jekyll?
jekyll 是一個靜態站點生成器，它會根據網頁源碼生成靜態文件。它提供了模板、變量、插件等功能，所以實際上可以用來編寫整個網站。所以用戶可以先在本地编写符合 Jekyll 规范的网站源码，然后上传到 github，由 github 生成并托管整个网站。

這種做法的好處是：

　　- 免費，無限流量。

　　- 享受 git 的版本管理功能，不用擔心文章遺失。

　　- 你只要用自己喜歡的編輯器寫文章就可以了，其他事情一概不用操心，都由 github 處理。

它的缺點是：

　　- 有一定技術門檻，你必須要懂一點 git 和網頁開發。

　　- 它生成的是靜態網頁，添加動態功能必須使用外部服務，比如評論功能就只能用 disqus。

　　- 它不適合大型網站，因爲沒有用到數據庫，每運行一次都必須遍曆全部的文本文件，網站越大，生成時間越長。

## Step 1 新建項目
```bash
mkdir jekyll_demo
cd jekyll_demo
git init
```

**然後，創建一個沒有父節點的分支 gh-pages。因爲 github 規定，只有該分支中的頁面，才會生成網頁文件。**
```bash
git checkout --orphan gh-pages
```
以下所有動作，都在該分支下完成。

## Step 2 創建設置文件
在項目根目錄下，建立一個名爲 _config.yml 的文本文件。它是 jekyll 的設置文件，我們在裏面填入如下內容，其他設置都可以用默認選項，具體解釋參見官方網頁。
```shell
baseurl: /jekyll_demo
```
目錄結構變成：
```shell
/jekyll_demo
　|--　_config.yml
```

## Step 3 創建模板文件
在項目根目錄下，創建一個 _layouts 目錄，用于存放模板文件。
```bash
mkdir _layouts
```
進入該目錄，創建一個 default.html 文件，作爲 Blog 的默認模板。並在該文件中填入以下內容。
```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="content-type" content="text/html; charset=utf-8" />
  <title>{{ page.title }}</title>
</head>
<body>
  {{ content }}
</body>
</html>
```
Jekyll 使用 Liquid 模板語言，{{ page.title }} 表示文章標題，{{ content }} 表示文章內容，更多模板變量請參考官方文檔。
目錄結構變成：
```shell
/jekyll_demo
　|--　_config.yml
　|--　_layouts
　 　　　|--　default.html

```
## step 4 創建文章
回到項目根目錄，創建一個_posts目錄，用于存放blog文章。
```shell
mkdir _posts
```
進入該目錄，創建第一篇文章。文章就是普通的文本文件，文件名假定爲2012-08-25-hello-world.html。**(注意，文件名必須爲"年-月-日-文章標題.後綴名"的格式。如果網頁代碼采用html格式，後綴名爲html；如果采用markdown格式，後綴名爲md。）**
在該文件中，填入以下內容：**（注意，行首不能有空格）**
```html
---
layout: default
title: 紙
---

<h2>{{ page.title }}</h2>
<p>快文</p>
<ul>{% for post in site.posts %}
  <li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}"></a></li>
  {% endfor %}
</ul>
```