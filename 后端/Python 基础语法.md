---
title: Python 基础语法
date: 2019-05-29 15:07:39
permalink: /pages/885e42/
tags: 
  - null
author: 
  name: evan
  link: https://github.com/hedongshu
comment: false
categories: 
  - Python
---
# Python 基础知识



Python是动态语言,强类型语言

​	不用事先声明类型，随时可以赋值为其他类型

​	不用类型之间必须先用 强制类型转换 为同一类型

## 语法

* 注释：# 标注的文字

```python
# 这是注释
```

* 缩进
  * 使用缩进表示层次关系
  * 约定使用4个空格
* 续行
  * 在行尾使用 `\`
```python
a = 'abc'
b = 'a\tb'
c = r'ab/cs\/cs'
```

* 标识符
  * 只能用 字母 数字 下划线组成，不能用数字开头
  * Python是大小写敏感
  * 约定： 不使用中文； 不使用歧义单词, 如class_； 不要随便使用下划线开头
  
* 常量
  
  * python无法定义常量
  
* 字面常量
  
  * 一个单独的量，如12、“abc”
  
* 变量
  
* 赋值之后，可以改变值的标识符
  
* 运算符

  * +-*/ 
  * 自然除法 / 结果是浮点数， 整除使用 // 

* 比较运算符

  * ==	!=	>	< 	>= 	<=

  * 返回一个bool值

    `1 == '1’` 返回的是False， 不会隐式转换
	* 链式比较
		`4>3>2`
	
* 逻辑运算符
  
* 与或非  and  or  not
  * 短路运算
  
    and  如果第一个表达式为Flase， 后面就不执行了，直接返回False
  
    or 如果第一个表达式为True， 后面就不执行了，直接返回True
  
* 赋值运算符
	* =
  
    ```python
    a = 'abc'
    b = a
    
    a += 'd'
    a = a + 'd'
    ```
  
  * 成员运算符
  
    in 、 not in
  
## 类型

* 数字
  * 整数： 不区分long int
    * 进制 0xa、0o10、0b10
    * bool :  true  false
  * 浮点型
    * 1.2 、3.14159、-0.12    科学计数法
    * 复数，  1+2j
```python
a = 1
b = 3.14
c = true
```
* 字符串
  * 使用 ` ‘’`   ` “”`  引用的字符序列
  
  * 使用 ` “””` ` ‘’‘` 引用的字符序列 ， 可以换行，可以自由的使用单双引号
  
  * 字符串前面加上 R 或者 r , 表示该字符串里面的内容不 转义   （转义用 `\`）  
  
  * 不可变 对象
  
    ```python
    s1 = 'string'
    s2 = 'str' + 'ing'  # 返回一个新的字符串
    ' '.join(s1)  # 用‘ ’分割字符， 返回一个新的字符串
    s1 = s1 * 3
    
    s3 = r'c:\windows\xx'
    s1[1] = 't'
    lst = list(s1)
    
    # 字符串分割
    # split(sep = None, maxsplit = -1) -> list of string
    s4 = 'i am groot'
    s4.split(' ')
    s4.rsplit(' ')  # 反方向分割
    
    s4.splitlines() # 按行分割
    
    # partition(sep) -> (head, sep, tail)
    # 从左到右，遇到分隔符就把字符串分割成两部分，  返回 头，分隔符，尾， 组成的元组，如果没有找到分隔符，就返回 头，空元素，空元素
    s4.partition('am')
    
    # 匹配替换
    'www.lalala.com'.replace('com', 'cn')
    
    ' asd '.strip() # 去除两端空白
    
    # 查找
    s4.find('am')
    s4.find('am', 1, 5)
    s4.rfind('am')
    
    # 判断
    s5 = ‘i am iron man’
    
    s5.startswith('i')
    s5.startswith('a', 2)
    s5.endswith('man')
    
    
    # 字符串格式化
    '{0}*{1}={2:<2}'.format(3, 2, 3*2)
    
    
    ```
  
    
  
  
  
* 列表

  * 使用 `[]` 表示

  * 列表内元素有顺序，可以使用索引

  * 元素可以是任意对象（数字，字符串，对象，列表等）

  * 线性的数据结构

    

  * 列表是可变的

  	

  * 列表list， 链表， queue， stack 的差异

  * 列表操作：

    ```python
    lst = []
    lst1 = list()
    lst2 = list(range(5))
    lst3 = [1, 2, 3]
    
    # 使用索引，不能越界
    # 正索引，从左至右，从0开始
    # 负索引，从右至左，从-1开始
    lst3[0] == 1
    lst3[-1] == 3
    
    # 列表查询, 返回匹配的位置， 可以使用负
    lst3.index(2) == 3
    lst3.index(1, -1) == 2
    
    # 返回列表中匹配的次数
    lst3.count(1) == 1
    
    # 列表修改
    lst3[1] = 1
    
    # 使用操作符
    lst3*2 == [1,2,3,1,2,3]  # 返回新列表
    lst2 + lst3 # 连接操作，返回新列表
    
    
    # 列表尾部追加元素;   返回值None，(表示没有新列表产生，在原列表修改，就地修改)
    lst3.append(obj)
    
    # 在指定位置插入， 返回None;  索引超上界，尾部追加，超下界，头部追加
    lst3.insert(index, obj)
    
    # 将可迭代对象追加进来， 返回None；
    lst3.extend(iteratable) 
    
    # 删除
    lst3.remove() # 返回None
    lst3.pop(index) # 不给index，默认弹出最后一个， 返回值是弹出的那个值
    lst3.clear() # 清除所有元素，剩下一个空列表， 返回None
    
    # 反转
    lst3.reversed() # 返回None
    
    # 排序 返回None
    # 默认对列表 升序排列； reverse为True，反转，降序；  key参数是一个函数，指定key如何排序
    lst3.sort(key=None, reverse=False)
    
    
    # 是否在列表内 , 返回bool
    [1] in [1, 2, 3]
    [5] not in [1, 2, 3]
    for x in [1, 2, 3]:
      pass
    
    # 列表复制, 返回一个新列表
    lst3.copy() 	# 浅拷贝
    copy.deepcopy(lst3) # 深拷贝
    
    
    ```

* 随机数

  * random 模块

    ``` python
    # random 模块
    
    random.randint(a, b)  #返回[a,b]之间的整数
    random.choice(seq) #从非空序列中随便挑选一个
    random.randrange(start, stop, step) #从指定范围内，按指定基数递增的集合中获取一个随机值，基数默认为1
    random.shuffle(list)  # 就地打乱顺序， 返回None
    ```



* 元组

  * tuple

  * 元组是 不可变 对象

  * 一个有序元素组成的集合

  * 使用小括号 `()` 表示

  * 支持索引（正负），不可以越界

    ```python
    t = tuple()
    t = ()
      
    t = tuple(range(1, 7, 2))  # iteratable
    t = (1,)
    
    t = (1,)*5
    
    # 查询
    t.index(value)
    t.count(value) # 返回列表中匹配的次数
    len(tuple)
    ```
    
  * 命名元组 namedtuple

##程序控制

- 顺序
  
  - 按照先后顺序一条条执行
- 分支
  - 分局不同情况判断，条件满足执行某条件下的语句
  
    * 单分支结构
  
      ```python
      # if语句
      if condition: 
          #代码块
      ```
  
      condition必须是一个bool类型，这地方有个隐式转换 bool(condition)
  
    * 多分支结构
  
      ```python
      # if ... elif ... else 语句
      if condition1:
      		# 代码块1
      elif condition2:
      		# 代码块2
      else:
      		# 代码块
      ```
- 循环
  
  - 条件满足就反复执行，不满足就不执行或不再执行
  
    ```python
    # for 循环
    for num in range(10)
    		print(num)
      
    # while 循环 
    flag = 10
    while flag != 0:
      	flag -= 1
    ```
  
    
