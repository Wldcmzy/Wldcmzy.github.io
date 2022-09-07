---
title: selenium爬虫的简单使用
date: 2022-09-07 08:44:35
categories:
  - [教练我想学挂边躲牛, 爬虫]
tags:
  -	爬虫
  - selenium
---

## 依赖

selenium模拟人使用浏览器，需要下载对应浏览器适合版本的驱动，放在与python.exe同目录

以Google举例: https://chromedriver.chromium.org/downloads

## 例子

Xpath,Css选择器路径 可以直接在浏览器F12右键复制

#### qq邮箱(点击头像)自动登录并发消息

```python
from selenium import webdriver
import  time
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import By
import os

driver = webdriver.Chrome()
driver.get('https://mail.qq.com')
time.sleep(2)
driver.maximize_window()
time.sleep(2)

driver.switch_to.frame("login_frame")
time.sleep(2)

driver.find_element(By.CSS_SELECTOR, "#img_out_？").click()
time.sleep(2)
driver.switch_to.default_content()
driver.find_element(By.CSS_SELECTOR, "#composebtn").click()
time.sleep(2)
driver.switch_to.frame('mainFrame')
time.sleep(2)
address=driver.find_element(By.CSS_SELECTOR, "#toAreaCtrl")
ActionChains(driver).click(address).send_keys("？@gmail.com").perform()
time.sleep(2)
subject=driver.find_element(By.CSS_SELECTOR, "#subject")
ActionChains(driver).click(subject).send_keys("123123").perform()

time.sleep(3)
mycontent=driver.find_element(By.CSS_SELECTOR, "#QMEditorArea > table > tbody > tr:nth-child(2) > td")

ActionChains(driver).click(mycontent).send_keys("abcdefghijklmn").perform()

driver.find_element(By.CSS_SELECTOR, '#toolbar > div > a:nth-child(1)').click()

os.system('pause')
```

