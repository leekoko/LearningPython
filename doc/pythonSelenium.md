# 【系列】Python+Selenium自动化测试框架

### 1.使用webdriver打开浏览器    

[geckodriver.exe](https://github.com/mozilla/geckodriver/releases)(火狐) / IEDriverServer.exe(IE) / chromedriver.exe(chrome) 放到python安装目录下      

```python
from selenium import webdriver   # 导入webdriver包  
driver = webdriver.Firefox()    # 初始化一个火狐浏览器实例：driver  
driver.maximize_window()        # 最大化浏览器  
driver.get("https://www.baidu.com")  # 通过get()方法，打开一个url站点  
driver.quit()     #关闭并退出浏览器  
```

- IE浏览器

  ```python
  driver = webdriver.Ie()  
  ```

  _报错的话需要把三个安全等级设为相同_   

- chrome浏览器

  ```python
  driver = webdriver.Chrome() 
  ```

1. 设定打开窗口大小   

   ```python
   driver.maximize_window()        # 最大化浏览器   

   driver.set_window_size(1280,800)  # 分辨率 1280*800  
   ```

   ​



### 2.XPATH定位技巧   

#### 1.XPATH+contains()   

eg:

```html
<a class="cate_menu_lk" href="//diannao.jd.com" target="_blank">电脑</a>
<span class="cate_menu_line">/</span>
<a class="cate_menu_lk" href="//channel.jd.com/670-716.html" target="_blank">办公</a>
```

使用：

``//*/a[contains(@href,'diannao')]``   contains用来判断href里面包含的关键字     

#### 2.XPATH相对定位    

eg:

```html
<input id="newtitle" type="radio" value="newstitle" name="tn"/>
<label class="not-checked" for="newstitle">新闻标题</label>
```

使用:

``.//*/label[@for='newstitle']/../input[@id='newstitle']``  前面的表示目标元素，后面的表示定位元素     



（feiman：XPATH需要进一步研究）    



### 3.自动化测试脚本

```python
# coding=utf-8  
import time  
from selenium import webdriver  
  
driver = webdriver.Chrome() # 打开chrome，如果没有安装chrome,换成webdriver.Firefox()  
driver.maximize_window()    # 最大化浏览器窗口  
driver.implicitly_wait(8)   # 设置隐式时间等待  
  
driver.get("https://www.baidu.com")  # 地址栏输入百度地址  
driver.find_element_by_xpath("//*[@id='kw']").send_keys("selenium")  # 搜索输入框输入Selenium  
driver.find_element_by_xpath("//*[@id='su']").click()  #点击百度一下按钮  
  
# 导入time模块，等待2秒  
  
time.sleep(2)   
# 这里通过元素XPath表达式来确定该元素显示在结果列表，从而判断Selenium官网这个链接显示在结果列表。  
# 这里采用了相对元素定位方法/../  
# 通过selenium方法is_displayed() 来判断我们的目标元素是否在页面显示。  
driver.find_element_by_xpath("//*[@id='1']/div[4]/div[1]/p[1]/a").is_displayed()  
driver.quit()  #退出浏览器
```

1. 等待是为了前面你的操作执行完，防止出错   

2. 需要copy chromeDriver.exe到Script文件夹下，chromeDriver的版本需要跟Chrome对应(版本对应自行搜索)，否则会出现”停止工作“问题。    

   下载chromeDriver位置：``http://chromedriver.storage.googleapis.com/index.html``    

3. 获取页面文字进行判断

   ```python
   # 第二个判断方法  
   ele_string = driver.find_element_by_xpath("//div/h3/a[text()='官网']/../a").text  
   if (ele_string == u"Selenium - Web Browser Automation"):  
       print "测试成功，结果和预期结果匹配！"  
   ```

4. 输入清除文字

   ```python
   driver.find_element_by_xpath("//*[@id='kw']").send_keys("selenium") #输入文字

   driver.find_element_by_id("kw").clear()    #清除文字  
   ```

5. 点击操作    

   ```python
   driver.find_element_by_xpath("//*[@id='su']").click()  #点击百度一下按钮 

   for i in driver.find_elements_by_xpath("//*/input[@type='radio']"):   #点击两个radio按钮   
       i.click()  
       
   driver.find_element_by_xpath("//*[@id='u1']/a[7]").click()        #点击复选checkbox按钮  
   time.sleep(1)  
   ```





### 4.其他定位技巧

1. 通过id定位    

   ```python
   try:  
       driver.find_element_by_id("kw")  
       print ('test pass: ID found')    
   except Exception as e:  
       print ("Exception found", format(e)) 
   ```

   添加了捕捉异常的操作   

2. 通过tag定位   

   ```python
   driver.find_element_by_tag_name("form")  
   ```

3. 用link text定位文字    

   ```python
   driver.find_element_by_link_text("新闻") 
   ```

4. 用partial link text定位部分文字   

   ```python
   driver.find_element_by_partial_link_text("主页")  
   ```

5. 用class定位   

   ```python
   driver.find_element_by_class_name("s_ipt")  
   ```

   不适用太长和可变化字符    

6. 用name定位   

   ```python
   driver.find_element_by_name("wd")
   ```

7. css定位（常用）    

   ```python
   driver.find_element_by_css_selector("#su")
   ```

   使用css的语法来定位，#su 表示 id = su   ,而上面通过id来定位是不需要加#的。

8. ​

   ​

### 5.正则表达式

爬取所有邮件地址   

```python
# coding=utf-8  
  
from selenium import webdriver  
import re  
  
driver = webdriver.Chrome()  
driver.maximize_window()  
driver.implicitly_wait(6)  
  
driver.get("http://home.baidu.com/home/index/contact_us")  
# 得到页面源代码  
doc = driver.page_source  
emails = re.findall(r'[\w]+@[\w\.-]+',doc) # 利用正则，找出 xxx@xxx.xxx 的字段，保存到emails列表  
# 循环打印匹配的邮箱  
for email in emails:  
    print (email)  
```

从源码中用正则表达式匹配出邮箱账号    

```python
pythonemails = re.findall(r'[\w]+@[\w\.-]+',doc)
```

1. 使用的是re模块   
2. \w表示：匹配包括下划线在内的任何字字符:[A-Za-z0-9_]   
3. +：匹配前一个字符1次或无限次   

### 6.页面其他操作   

#### 1.刷新页面   

```python
driver.get("https://www.baidu.com")  
time.sleep(2)  
try:  
    driver.refresh() # 刷新方法 refresh  
    print ('test pass: refresh successful')  
except Exception as e:  
    print ("Exception found", format(e))  
driver.quit()  
```

#### 2.后退前进操作   

```python
driver.get("https://www.baidu.com")  
time.sleep(2)  
elem_news = driver.find_element_by_link_text("新闻")    
elem_news.click() # 点击进入到百度新闻  
time.sleep(2)  
driver.back()  # 从百度新闻后退到百度首页  
time.sleep(2)  
driver.forward() # 百度首页前进到百度新闻  
time.sleep(2)  
driver.quit()  
```

#### 3.获取当前页面的url   

```python
print (driver.current_url)  # current_url 方法可以得到当前页面的URL  
```

获取的URL可以帮助我们判断跳转的页面是否正确    

相似作用的还有获取title：

```python
print (driver.title)  # title方法可以获取当前页面的标题显示的字段  
```

#### 4.触发键盘按钮    

```python
driver.get("http://www.baidu.com/")  
time.sleep(1)  
ele = driver.find_element_by_tag_name('body').send_keys(Keys.CONTROL + 't')  # 触发ctrl + t  
time.sleep(1)  
```

打开新的tab前，要模拟人类操作，适当停一停    









http://blog.csdn.net/u011541946/article/details/69694510

































































