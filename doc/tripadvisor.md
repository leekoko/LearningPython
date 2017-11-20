# 爬取tripadvisor网站   

## 1.页面A爬取源码

```python
from bs4 import BeautifulSoup
import requests

url = 'https://www.tripadvisor.cn/Attractions-g60763-Activities-New_York_City_New_York.html'
wd_data = requests.get(url)
soup = BeautifulSoup(wd_data.text,"lxml")
titles = soup.select("div.listing_title > a[target='_blank']")
imgs = soup.select('img[width="180"]')  #图片做了反爬处理，通过js加载出来
cates = soup.select('div.p13n_reasoning_v2')

for title,img in zip(titles,imgs):
    data = {
        'title':title.get_text(),   #获取文本内容
        'img':img.get('src'),     #获取src的内容
    }
    print(data)
```

### 1.用request请求

可以向目标网站发出请求，还可以发出post请求：``r = requests.post("http://httpbin.org/post")``   

还可以传参,设置编码   

```python
>>> payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
>>> r = requests.get('http://httpbin.org/get', params=payload)

>>> r.encoding = 'ISO-8859-1'
```

_Http是一种标准，数据只要遵守这套行为准则，就可以在不同的计算机间跑来跑去。而其中有两条规则就叫做get请求，post请求。这两条规则主要就是用来告诉数据：你接下来要去哪个计算机上。_

### 2.包装进BeautifulSoup里面，从BeautifulSoup中提取信息  

_BeautifulSoup是一个智能的处理工具，用户只要将抓取到的网页源码交给它，然后告诉它：我要img标签，而且只要宽度为180的img标签。它就会直接把符合的标签拿给你，当说：我只要里面的文字，它也会会很智能地把文字提取出来。_  

### 3.将内容存进data里面   

_当我们把信息抽取出来后，他们就是单纯的一整块的信息。而传输他们一般都要你把他们整理好再给它。整理的模板就是“标签 - 内容”。所以我们就可以把所有的图片的地址添加img的标签，以此来存储。_







课时11   23min