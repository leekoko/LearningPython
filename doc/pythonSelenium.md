# 【系列】Python3碎片基础

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

1. 添加标签

   ```python
   driver.get("http://www.baidu.com/")  
   time.sleep(1)  
   ele = driver.find_element_by_tag_name('body').send_keys(Keys.CONTROL + 't')  # 触发ctrl + t  
   time.sleep(1)  
   ```

   打开新的tab前，要模拟人类操作，适当停一停    

2. 全选当前页面的文字    

   ```python
   element = driver.find_element_by_tag_name('body')  
   element.send_keys(Keys.CONTROL + 'a')  
   ```


3. 全选删除文字    

   ```python
   element.send_keys(Keys.CONTROL+'a')  
   element.send_keys(Keys.BACKSPACE)  
   ```

4. 右键图像

   ```python
   element = driver.find_element_by_xpath("//*[@id='lg']/img")  
   actionChains = ActionChains(driver)  
   actionChains.context_click(element).perform()
   ```

   ActionChains用来右键目标。

#### 5.执行js代码      

```python
driver.execute_script("window.alert('这是一个alert弹框。');") #直接插入js执行

# 使用js做滚动
driver.execute_script("return arguments[0].scrollIntoView();",target_elem) # 用目标元素参考去拖动 
# 随便的拖动
driver.execute_script("scroll(0,2400)") # 这个是第二种方法，比较粗劣，大概的拖动  
```

#### 6.切换窗口   

```python
import time  
from selenium import webdriver  
  
driver = webdriver.Chrome()  
driver.get('http://news.baidu.com')  
time.sleep(1)  
  
driver.find_element_by_xpath("//*[@id='pane-news']/div/ul/li[1]/strong/a").click()  
print (driver.current_window_handle) # 输出当前窗口句柄  
handles = driver.window_handles # 获取当前全部窗口句柄集合  
print (handles) # 输出句柄集合  
  
for handle in handles:# 切换窗口  
    if handle != driver.current_window_handle:  
        print ('switch to second window',handle)  
        driver.close() # 关闭第一个窗口  
        driver.switch_to.window(handle) #切换到第二个窗口  
```

执行点击之后，核心的切换窗口代码只有：

```python
driver.find_element_by_xpath("//*[@id='pane-news']/div/ul/li[1]/strong/a").click()  
handles = driver.window_handles # 获取当前全部窗口句柄集合

for handle in handles:# 切换窗口 
        driver.close() # 关闭第一个窗口  
        driver.switch_to.window(handle) #切换到第二个窗口  
```

### 7.方法常用

#### 1.断言

所谓的断言作用是测试程序在我的假设条件下，是否能够正常良好的运作。assert与if的作用相似。

```python
# coding=utf-8  
import time  
from selenium import webdriver  
  
driver = webdriver.Chrome()  
#driver.maximize_window()  
driver.get('https://www.baidu.com')  
time.sleep(1)  
# 方法一  
try:  
    assert "百度一下" in driver.title  
    print ("Assertion test pass.")  
except Exception as e:  
    print ("Assertion test fail.", format(e))  
  
# 方法二  
if "百度一下，你就知道" == driver.title :  
    print ("Assertion test pass.")  
else:  
    print ("Assertion test fail.")  
  
print (driver.title)  
```

没法设置窗口大小，可能是插件与系统位数不兼容。

#### 2.获取属性的方法   

1. 获取文本

```python
# 断言方法二，本文重点介绍方法  
error_mes = driver.find_element_by_xpath("//*[@id='TANGRAM__PSP_8__error']").text  
try:  
    assert error_mes == u'请您填写手机/邮箱/用户名'  
    print ('Test pass.')  
except Exception as e:  
    print ("Test fail.", format(e))  
```

2. 获取id

```python
print (link.get_attribute('id'))  
```

3. 获取href

```python
print (link.get_attribute('href'))  
```



#### 3.is_selected判断选中方法   

```python
try:  
    driver.find_element_by_xpath("//*[@id='news']").is_selected()  
    print ('Test Pass.')  
except Exception as e:  
    print ('Test fail',format(e))  
```

#### 4.切换iframe   

定位没问题，但就是获取不到元素。查看开发者工具，火狐可以判断其是否为Top Window，不是的话需要对iframe进行切换    

```python
driver.switch_to.frame("iframe1")  
```

#### 5.截图页面   

像遇到错误的情况，就可能需要截图   

```python
# coding=utf-8  
import time  
from selenium import webdriver  
  
driver = webdriver.Chrome()  
driver.implicitly_wait(6)  
driver.get("https://www.baidu.com")  
time.sleep(1)  
  
driver.get_screenshot_as_file("D:\\baidu.png")  
driver.quit()  
```

分隔符 \ 需要变成 \ \

### 8.基础语法   

#### 1.类与方法   

Demo 1：  类与方法介绍

```python
# coding=utf-8  
  
class ClassA(object):  
  
    string1  = "这是一个字符串。"  
  
    def instancefunc(self):  
        print ('这是一个实例方法。')  
        print (self)  
 
    @classmethod  
    def classfunc(cls):  
        print ('这是一个类方法。')  
        print (cls)  
 
    @staticmethod  
    def staticfun():  
        print ('这是一个静态方法。')  
  
  
test = ClassA()  # 初始化一个ClasssA的对象，test是类ClassA的实例对象  
test.instancefunc()  # 对象调用实例方法  
  
test.staticfun()  # 对象调用静态方法  
  
test.classfunc()  # 对象调用类方法  
  
print test.string1 # 对象调用类变量  
  
ClassA.instancefunc(test)  # 类调用实例方法，需要带参数，这里的test是一个对象参数  
ClassA.instancefunc(ClassA) # 类调用实例方法，需要带参数，这里的ClassA是一个类参数  
ClassA.staticfun() # 类调用静态方法  
ClassA.classfunc()  # 类调用类方法  
```

Demo 2：类方法调用   

```python
# coding=utf-8  
import time  
from selenium import webdriver  
  
class BaiduSearch(object):  
    driver = webdriver.Chrome()  
    driver.maximize_window()  
    driver.implicitly_wait(10)  
  
    def open_baidu(self):  
        self.driver.get("https://www.baidu.com")  
        time.sleep(1)  
  
    def test_search(self):  
        self.driver.find_element_by_id('kw').send_keys("selenium")  
        time.sleep(1)  
        print self.driver.title  
        try:  
            assert 'selenium' in self.driver.title  
            print ('Test pass.')  
  
        except Exception as e:  
            print ('Test fail.')  
        self.driver.quit()  
  
baidu = BaiduSearch()  
baidu.open_baidu()  
baidu.test_search()  
```

Demo 3：调用截取图片方法

```python
class BasePage(object):     
    def take_screenshot(self):  
        """ 
        截图并保存在根目录下的Screenshots文件夹下 
        :param none: 
        """  
        file_path = os.path.dirname(os.getcwd()) + '/Screenshots/'  
        rq = time.strftime('%Y%m%d%H%M%S',time.localtime(time.time()))  
        screen_name = file_path + rq + '.png'  
        try :  
            self.driver.get_screenshot_as_file(screen_name)  
            mylog.info("开始截图并保存")  
  
        except Exception as e:  
            mylog.error("出现异常",format(e)) 
```

```python
# coding=utf-8  
import time  
from selenium import webdriver  
from test.basepage import BasePage  
  
class TestScreenshot(object):  
    driver = webdriver.Chrome()  
    driver.maximize_window()  
    driver.implicitly_wait(10)  
    basepage = BasePage(driver)  
  
    def test_take_screen(self):  
        self.basepage.open_url("https://www.baidu.com")  
        time.sleep(1)  
        self.basepage.take_screenshot()  
        self.basepage.quit_browser()  
  
test = TestScreenshot()  
test.test_take_screen()  
```

#### 2.操作时间

```python
# coding=utf-8  
import time  

class GetTime(object):  
    def get_system_time(self):  
        print (time.time()) # time.time()获取的是从1970年到现在的间隔，单位是秒  
        print (time.localtime())  
        new_time = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime()) # 格式化时间，按照 2017-04-15 13:46:32的格式打印出来  
        print (new_time)  
  
gettime = GetTime()  #类
gettime.get_system_time()   #方法  
```

#### 3.IO文件

1. 写入

   ```python
   text = "Sample Text to Save \nNew Line"  
     
   ''''' 
   调用buid-in函数：open打开或者创建文件， 
   如果exampleFile.txt不存在，就自动创建 
   w在这里表示可以写的模式，如果是读那就'r' 
   '''  
   saveFile = open('exampleFile.txt', 'w')  
   saveFile.write(text)    #追加
   saveFile.close()  # 操作完文件后一定要记得关闭，释放内存资源  
   ```

2. 读取

   ```python
   readMe = open('exampleFile.txt','r').read()  
   print(readMe)  
   ```

#### 4.获取键盘输入    

```python
x = input('What is your name?')  
print('Hello',x)  
```























书籍：XPATH的使用方式































































