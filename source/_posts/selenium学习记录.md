title: selenium
date: 2016-09-22 21:54:02
tags: [selenium,python]
categories: [selenium,python]
---
1.鼠标悬停的实现方式 
  >a) 
  ```python
  test = browser.find_element_by_id("qmenu")
  hover = ActionChains(browser).move_to_element(test)
  hover.perform()
  ```

  >b)
```python
from selenium.webdriver.common.action_chains import ActionChains
def hover(self):
    wd = webdriver_connection.connection
    element = wd.find_element_by_link_text(self.locator)
    hov = ActionChains(wd).move_to_element(element)
    hov.perform()
```
2.翻页功能
```python
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
```
