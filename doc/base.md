# Python入门    

## 1.环境搭建   

### 1.安装python   

我使用的是python-3.6.3-amd64-webinstall.exe,安装完之后还需要添加环境变量，在path中添加python的安装路径和pip的路径(在python的Scripts里面)，以dos命令下运行``python``不报错来确认安装成功。      

### 2.安装IDE  

 我使用的是JetBrains PyCharm，安装过程比较缓慢   

 破解网站：``http://idea.liyang.io/``   

### 3.安装库  

库的地址：``https://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml``   

1. beautifulsoup4-4.5.3   

   安装方式：

   1. copy文件导python安装目录下   
   2. 使用dos命令cd导beautifulsoup4目录下   
   3. 执行 ``python setup.py install``安装命令       

2. request    

   解压request文件，将其copy到python的lib下完成安装   

3. urllib3

4. chardet   

5. certifi   

6. idna   

7. lxml   

8. tornado  


安装是否成功以导入是否报错为准   

## 2.网站爬取   

### 1.获取网页源码   

```python
from bs4 import BeautifulSoup
import requests

url = 'http://opac.gzlib.gov.cn/opac/search?q=A1&searchType=standard&isFacet=false&view=simple&searchWay=class&rows=10&sortWay=score&sortOrder=desc&libcode=GT&searchWay0=marc&logical0=AND&page=6'
wb_data = requests.get(url)
soup = BeautifulSoup(wb_data.text,'lxml')

print(soup)
```

### 2.获取指定标签对象   

```python
titles = soup.select('div.bookmeta > div > span > a')
print(titles)
```

### 3.遍历获取标签内容   

```python
for title in titles:
    print(title.get_text())
```

### 4.去掉\r，\n，\t ，空行    

```python
    title = title.get_text().replace('\t','').replace('\n','').replace('\r','')
```

```python
    if(author==''):
        continue;
```

### 5.遍历多个参数   

```python
#所有信息进行打包
for title,author in zip(outBlankStrs_title,outEvens_author):
```

### 6.取奇数行   

```python
for index,outBlankStr in enumerate(outBlankStrs_author):
    if index % 2 == 1:
        continue
```

### 7.存入数组  

```python
#处理标题内容
outBlankStrs_title = []
for title in titles:
    title = title.get_text().replace('\t','').replace('\n','').replace('\r','')
    outBlankStrs_title.append(title)
```

### 8.获取动态加载数据   









11课后续：request伪造登陆信息    

  







