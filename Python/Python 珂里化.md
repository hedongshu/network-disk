# Python 珂里化

- 指 将原来接受两个参数的函数，变成接受一个参数的函数的过程。 新的函数返回一个以原来第二个参数为参数的函数

- `z =f(x, y)` 转换成`z=f(x)(y)`

- 栗子

  ```python
  def add(x, y):
  	return x + y


  def newAdd(x):
  	def inner(y):
      return x + y
    return inner()

  ```
