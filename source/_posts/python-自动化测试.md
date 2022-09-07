---
title: python 自动化测试
date: 2022-09-07 09:19:53
categories:
  - [基础知识, python, unittest]
tags:
  -	自动化测试
  - unittest
  - 测试
---

```python
import unittest

class  Task_B(unittest.TestCase):
    #初始化类
    def setUp(self):
        #在执行任何方法之前，都先执行setup方法
        print('Initial Task B')
        
    #挨个执行以test开头的方法
    def test_002(self):
        print('Task B test 2')

    def  test_001(self):
        print('Task B test 1')

    def tearDown(self):
        print('Task B Teardown')
    
    def NO(self):
        print('this method can\'t be executed')

class  Task_A(unittest.TestCase):
    #初始化类
    def setUp(self):
        #在执行任何方法之前，都先执行setup方法
        print('Initial Task A')
        
    #挨个执行以test开头的方法
    def test_002(self):
        print('Task A test 2')

    def  test_001(self):
        print('Task A test 1')

    def tearDown(self):
        print('Task A Teardown')
          
if __name__ == '__main__':
    unittest.main()
    print('发现按字典序执行')
```

