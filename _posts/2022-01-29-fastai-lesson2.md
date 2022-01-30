---
layout: single
title:  "fast.ai 学习 第二课笔记"
date:   2022-01-29 16:13:51 +0800
categories:
  - fast.ai
tags:
  - 笔记
  - 白嫖
  - Unsplash
---

## 1. 替换Bing API
好家伙，上来就是一个大活，Bing API这位更是重量级，要Azure注册，Azure注册需要Visa卡……

我老白嫖党了，能受你们这个气？
直接去找其他的图片引擎

搜索了一下，unsplash这个API不错，Restful的。

1. 在[这里](https://unsplash.com/developers)注册，只需要邮件就行
2. 注册后新建个Application，点[这里](https://unsplash.com/oauth/applications/new)也行
3. 拉到下方，拿Access Key：
![unsplash_ak](/assets/images/unsplash_ak.jpg)

拿到直接写个函数替换，Unsplash API一次请求最多返回30个数据，请求5次，直接平替Bing API的150个数据：

添加函数：

```python
import requests

def request_images_unsplash_per_page(key, keyword, page):
    params = {"query": keyword, "per_page": 150, "page": page}
    headers = {"Authorization": f"Client-ID {key}"}
    response = requests.get("https://api.unsplash.com/search/photos", params=params, headers=headers)
    return response.json()

def search_images_unsplash(key, keyword):
    urls = []
    for i in range(5):
      json_result = request_images_unsplash_per_page(key, keyword, i)
      for result_item in json_result["results"]:
            urls.append(result_item["urls"]["small"])
    return urls

key = "XXX" #这里填写你刚刚申请的Access Key
```

再将原本的下载函数进行替换：

```python
if not path.exists():
    path.mkdir()
    for o in bear_types:
        dest = (path/o)
        dest.mkdir(exist_ok=True)
        # results = search_images_bing(key, f'{o} bear')
        # download_images(dest, urls=results.attrgot('contentUrl'))
        urls = search_images_unsplash(key, f'{o} bear')
        download_images(dest, urls=urls)
```

最后检查一下，没毛病。白嫖，yyds：
![unsplash_result](/assets/images/unsplash_result.jpg)