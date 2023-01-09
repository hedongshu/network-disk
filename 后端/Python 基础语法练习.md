---
title: Python 基础语法练习
date: 2019-05-31 10:30:10
permalink: /pages/4a39cd/
tags: 
  - null
author: 
  name: evan
  link: https://github.com/hedongshu
comment: false
categories: 
  - Python
---
# Python 基础语法练习



```python
!/usr/bin/env python3
-*- coding: utf-8 -*-

# 打印空心矩形
def drawRect(x, y):
    for j in range(y):
        text = ''
        for i in range(x):
            if(j == 0 or j == y-1):
                text += '*'
            else:
                if(i == 0 or i == x - 1):
                    text += '*'
                else:
                    text += ' '
        print(text)

# 100以内奇数的和
def sum():
    sum = 0
    for i in range(0, 100, 2):
        return sum += i

# 求阶乘
def factorial(n):
    if (n == 1):
        return 1
    else:
        return n * factorial(n - 1)


# 打印九九乘法表
def printjiujiu():
    for i in range(0, 9):
        text = ''
        for j in range(0, i + 1):
            text += str(j + 1) + '*' + str(i + 1) +\
                '=' + str((i + 1) * (j + 1)) + ' '
        print(text)


# 打印菱形
def pritnt2(n):
    for i in range(-n//2, n//2+1):
        print(' ' * abs(i) + '*'*(n - 2 * abs(i)))


# 打印闪电
n = 12

for i in range(-n//2, n//2 + 1):
    if i < 0:
        print(' ' * (abs(i)+1) + '*' * (n//2 + 1 - abs(i)))
    elif i > 0:
        print(' '*abs(n//2 - 1) + '*' * (n//2 + 1 - abs(i)))
    else:
        print(' ' + '*'*(n))
        print('*'*(n))


# 斐波那契数列
def fb(num):
    thisList = []
    a = 0
    b = 1
    for i in range(1, num + 1):
        thisList.append(b)

        c = a + b
        a = b
        b = c
    print(b)


# 十万以内素数
import math
# 判断是否素数
def isPrime(num):
    s = int(math.sqrt(num))+1
    for i in range(2, s):
        if num % i == 0:
            return False
    else:
        return True

def prime(max):
    primeList = []
    for i in range(1, max + 1, 2):
        if(isPrime(i)):
            primeList.append(i)
    print(primeList)


# 猴子吃桃
n = 1
for i in range(9):
    n = (n + 1) * 2

print(n)
 
```

