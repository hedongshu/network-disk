---
title: python 函数，参数，参数解构
date: 2019-06-03 13:47:19
permalink: /pages/0eb873/
tags: 
  - null
author: 
  name: evan
  link: https://github.com/hedongshu
comment: false
categories: 
  - Python
---
# python 函数



##  函数的作用

* 结构化编程对代码的最基本封装
* 为了复用，减少冗余代码
* 代码更加简洁美观，可读易懂



## 函数的分类

* 内建函数：max()  reversed()  等
* 库函数： math.ceil()  等



## 函数定义

```python
# 定义
def sum(x, y):
  return x + y


#调用
sum(1， 2)

```

* 函数名就是标识符，命名要求一样
* 语句块必须缩进，约定4个空格
* 没有return语句，会返回一个None



# 函数参数

* 位置参数

  * `def f (x,y,z)` 调用使用 `f(1, 2, 3)`
  * 按照参数定义顺序传入实参

* 关键字参数

  * `def f (x, y, z)` 调用使用 `f(x=1, y=2, z=3)`
  * 参数指定形参名字，传参顺序可以与定义顺序不同

* 传参

  ```python
  f(z=None, y=10, x=[1])
  f((1,), z=6, y=4.1)
  ```

  

* 参数默认值

  参数默认值可以在未传入足够参数的时候，自动使用默认参数

  ```python
  def sum (x=2, y=3):
  	return x+y
  
  # 调用
  sum()
  sum(1)
  sum(y=5 )
  ```

  

* 可变参数

  一个形参可以匹配任意个参数

  ```python
  def add(*nums):
    sum = 0
    print(type(nums))  # nums会被拼成一个tuple
    for x in nums:
      sum += x
    return sum
  
  add(1, 2, 3, 4)
  ```

  

  可变参数使用关键字

  ```python
  def showconfig(**kwargs):
    for k,v in kwargs.items():
      print(k, y)
      
   
  showconfig(host='asdf', port='asdfsaf')
  ```

  

  可变参数混合使用

  ```python
  # 声明
  def show(name, pw, **kwargs)
  # 调用
  show('tom', '1234', port='8080')
  show('tom', pw='1234', port='8080', host='127,0,0.1')
  
  
  # 声明
  def show(name, *args, **kwargs)
  # 调用
  show('tom', '123','asdf', port='8080')
  show('tom', '123', port='8080', host='127.0.0.1')
  
  
  ```




# 参数解构

```python
def add(x, y):
  return x + y

# 调用
add(1, 2)

t = (2, 3)
l = [4, 5]

add(*t)
add(*t)
add(*range(1, 3))

d = {'x' = 1, 'y' = 2}

add(**d)

```



# 高阶函数

* 定义

  接受一个或者多个函数作为参数

  or  返回一个函数

  ```python
  # 计数器
  def counter(base):
    def inc(step=1)
    	base += step
      return bask
   	return inc
  
  foo = counter(5)
  foo()
  ```

* 匿名函数

  Python的lambda表达式基本语法是在冒号（：）左边放原函数的参数，可以有多个参数，用逗号（，）隔开即可；冒号右边是返回值。

  ```python
  fun = lambda x, y : a > b
  fun(1, 2)
  
  
  # 排序
  def mysort(lst, fun=lambda a, b: a > b):
      newList = []
      for i in lst:
          for idx, val in enumerate(newList):
              if fun(i, val):
                  newList.insert(idx, i)
                  break
          else:
              newList.append(i)
      return newList
  
  
  lst = [1, 5, 2, 54, 12, 65, 2]
  print(mysort(lst, lambda a, b: a < b))
  
  ```



 * 偏函数

   `partial()`可以设定参数的默认值，降低调用函数的难度

   ```python
   import functools
   # 
   int2 = functools.partial(int, base=2)
   
   int2('1000000')
   
   int2('1010101')
   ```

   

# 闭包

```python
def createCounter():
    num = 0

    def counter():
        nonlocal num
        num += 1
        return num

    return counter


counter = createCounter()

print(counter())
print(counter())
print(counter())
```



# 装饰器

装饰器本质上是返回一个函数的高阶函数

```python
@log
def now():
    print('2015-3-25')
```

把`@log`放到`now()`函数的定义处，相当于执行了语句：

```python
now = log(now)
```

如果装饰器需要带参数

```python
@log('execute')
def now():
    print('2015-3-25')
    
 # 相当于下面

now = log('execute')(now)
```



* 栗子

```python
import time
import functools


def metric(arg='exected in'):
    if not isinstance(arg, str):
        @functools.wraps(arg)
        def wrapper(*args, **kw):
            st = time.time()
            reject = arg(*args, **kw)
            et = time.time()
            print('{}{}{}' .format(arg.__name__, 'exected in', et - st))
            return reject
        return wrapper
    else:
        def decorator(fun):
            @functools.wraps(fun)
            def wrapper(*args, **kw):
                st = time.time()
                reject = fun(*args, **kw)
                et = time.time()
                print('{}{}{}' .format(fun.__name__, arg, et - st))
                return reject
            return wrapper
        return decorator


# 测试
@metric
def fast(x, y):
    time.sleep(0.0012)
    return x + y


@metric('xixixi')
def slow(x, y, z):
    time.sleep(0.1234)
    return x * y * z


print(fast(11, 22))
print(slow(11, 22, 33))

```



