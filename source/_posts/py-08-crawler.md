---
title: Python 08 - 用 Python 抓网页
date: 2017-10-09
tags: [Python, 爬虫]
categories:
  - 编程语言
  - Python
---

## 为什么 Python 适合写爬虫

库成熟、代码简洁，十几行就能跑起来。2017年主流方案是 `requests` + `BeautifulSoup`。

先安装：

```bash
pip install requests beautifulsoup4
```

## requests — 发 HTTP 请求

```python
import requests

# GET
response = requests.get("https://httpbin.org/get")
print(response.status_code)   # 200
print(response.text)           # 响应体（字符串）
print(response.json())         # 自动解析 JSON

# 带参数的 GET（自动拼接到 URL）
params = {"q": "python", "page": 1}
r = requests.get("https://example.com/search", params=params)

# POST
data = {"username": "alice", "password": "123"}
r = requests.post("https://example.com/login", data=data)

# 设置请求头（伪装成浏览器）
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
}
r = requests.get("https://example.com", headers=headers)

# 设置超时，避免卡死
r = requests.get("https://example.com", timeout=5)
```

## BeautifulSoup — 解析 HTML

```python
from bs4 import BeautifulSoup

html = """
<html>
  <body>
    <h1 class="title">Python 爬虫</h1>
    <ul id="links">
      <li><a href="/page1">第一页</a></li>
      <li><a href="/page2">第二页</a></li>
    </ul>
  </body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")

# 查找元素
soup.find("h1")                        # 第一个 h1 标签
soup.find("h1", class_="title")        # 指定 class
soup.find(id="links")                  # 指定 id
soup.find_all("a")                     # 所有 a 标签（返回列表）
soup.find_all("a", limit=5)            # 最多找 5 个

# 取内容
h1 = soup.find("h1")
h1.text          # "Python 爬虫"（标签内文字）
h1.get("class")  # ["title"]（取属性）

# 取所有链接
for a in soup.find_all("a"):
    print(a.text, a.get("href"))
```

## 实战：抓一个页面

```python
import requests
from bs4 import BeautifulSoup

def fetch_links(url):
    headers = {"User-Agent": "Mozilla/5.0"}
    try:
        r = requests.get(url, headers=headers, timeout=10)
        r.raise_for_status()   # 如果状态码不是 2xx，抛出异常
        r.encoding = "utf-8"
    except requests.RequestException as e:
        print(f"请求失败：{e}")
        return []

    soup = BeautifulSoup(r.text, "html.parser")
    links = []
    for a in soup.find_all("a", href=True):
        links.append(a["href"])
    return links

links = fetch_links("https://example.com")
for link in links[:10]:
    print(link)
```

## 保存数据

```python
import csv, json

data = [
    {"title": "文章一", "url": "/article/1"},
    {"title": "文章二", "url": "/article/2"},
]

# 保存为 JSON
with open("result.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

# 保存为 CSV
with open("result.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["title", "url"])
    writer.writeheader()
    writer.writerows(data)
```

## 常见坑

- 很多网站反爬，先加 `User-Agent`，再不行加 Cookie 或延迟请求
- 用 `r.raise_for_status()` 检查请求是否成功，别只判断 `status_code == 200`
- 网页编码不对会出乱码，试试手动设置 `r.encoding = "utf-8"`
- 爬虫要遵守网站的 `robots.txt`，不要高频请求把人家服务器打挂
- 解析中文时用 `html.parser` 或 `lxml`（需要额外安装），不要用 `html5lib`（慢）
