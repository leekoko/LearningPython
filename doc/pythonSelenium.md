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

### 2.XPATH定位技巧   

http://blog.csdn.net/u011541946/article/details/67639423













