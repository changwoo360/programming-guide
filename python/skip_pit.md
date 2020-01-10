## 不要同时使用readlines和readline

```python
# 不要同时使用readlines和readline
with open(file, "r") as f:
    # 读取所有的行并写入到列表中
		f.readlines()
		# 此处存在一个坑，使用readlines方法后，再次使用readline将无法读取到内容
		# 原因：无论是readlines还是readline，都会读取到内容。
		# 尤为重要的是，该方法会占用文件的内容，导致无法读取重复文件。
		# 这不是bug，这是readline的设计原理
		f.readline()  # None
```

## 避免循环累加
```python
Yes: items = ['<table>']
     for last_name, first_name in employee_list:
         items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
     items.append('</table>')
     employee_table = ''.join(items)
```
```python
No: employee_table = '<table>'
    for last_name, first_name in employee_list:
        employee_table += '<tr><td>%s, %s</td></tr>' % (last_name, first_name)
    employee_table += '</table>'
```
避免在循环中用+和+=操作符来累加字符串. 由于字符串是不可变的, 这样做会创建不必要的临时对象, 并且导致二次方而不是线性的运行时间. 作为替代方案, 你可以将每个子串加入列表, 然后在循环结束后用 .join 连接列表. (也可以将每个子串写入一个 cStringIO.StringIO 缓存中.)
```python
import timeit

def func1():
	"""不推荐方式"""
	content = "<table>"
	for i in range(10000):
		content += "<a href='www.baidu.com'>baidu</a>"
	content += "</table>"


def func2():
	"""推荐方式"""
	content = ["<table>", ]
	for i in range(10000):
		content.append("<a href='www.baidu.com'>baidu</a>")

	content.append("</table>")

	content = "".join(content)


if __name__ == '__main__':

	# t = timeit.timeit(stmt="func1()", setup="from __main__ import func1", number=1000)
	# t = timeit.timeit(stmt="func2()", setup="from __main__ import func2", number=1000)
	print(t)
	# func2的效率比func1的效率要高1/3
```

## 不要在函数和方法中定义可变对象作为默认值  
```python
Yes: def foo(a, b=None):
         if b is None:
             b = []
```
```python
No:  def foo(a, b=[]):
         ...
No:  def foo(a, b=time.time()):  # The time the module was loaded???
         ...
No:  def foo(a, b=FLAGS.my_thing):  # sys.argv has not yet been parsed...
         ...
```
**该地方有一个坑，当定义可变对象作为参数默认值时，会出现问题。**  
```python
# 错误的参数
def func(numbers=[], num=1):
    numbers.append(num)
    return numbers

if __name__ == "__main__":
    for i in range(3):
        print(func())

"""和预想的不一样
[1]
[1, 1]
[1, 1, 1]
"""
```
```python
# 正确的参数
def func2(numbers=None, num=1):
    numbers = numbers if numbers else []
    numbers.append(num)
    return numbers

if __name__ == "__main__":
    for i in range(3):
        print(func2())

"""这次和预想的结果一致
[1]
[1]
[1]
"""
```
**为什么会出现这种情况呢？**  
其实是因为.py文件运行时会生成一个.pyc文件，该文件会加载所有的函数和函数预设的属性（不调用该函数）。导致了numbers被写入到了文件中，相当于全局变量。又因为该变量为可变数据类型，导致不会重新赋值，使用的是被.pyc"缓存"起来的numbers，最终导致执行结果与预想的不一致。

**但它并非是Python的坑，而是出于某种实际设计考虑，有其应用场景。**  
例如：计算一个数的阶乘时可以用一个可变对象的字典当作缓存值来实现缓存，缓存中保存计算好的值，第二次调用的时候就无需重复计算，直接从缓存中拿。
```python
def factorial(num, cache={}):
	if num == 0:
		return 1
	if num not in cache:
		print('xxx')
		cache[num] = factorial(num - 1) * num
		return cache[num]
	print(factorial(4))
	print("-------")
	print(factorial(4))

"""第二次调用的时候，直接从 cache 中拿了值。并且，执行速度会快很多。也可以做装饰器使用
---第一次调用---
xxx
xxx
xxx
xxx
24
---第二次调用---
24
"""
```
参考文章：
- [Python为何不能用可变对象作为默认参数的值](https://www.jb51.net/article/164314.htm)  

def是一条可执行语句，Python 解释器执行 def 语句时，就会在内存中就创建了一个函数对象（此时，函数里面的代码逻辑并不会执行，因为还没调用嘛），在全局命名空间，有一个函数名（变量叫 func）会指向该函数对象，记住，至始至终，不管该函数调用多少次，函数对象只有一个，就是function object，不会因为调用多次而出现多个函数对象。

函数对象生成之后，它的属性：名字和默认参数列表都将初始化完成。

初始化完成时，属性 __default__ 中的第一个默认参数 numbers 指向一个空列表。当函数第一次被调用时，就是第一次执行 func()时，开始执行函数里面的逻辑代码（此时函数不再需要初始化了），代码逻辑就是往numbers中添加一个值为1的元素。

第二次调用 func()，继续往numbers中添加一个元素。

第三次、四次依此类推。所以现在你应该明白为什么调用同一个函数，返回值确每次都不一样了吧。因为他们共享的是同一个列表(numbers)对象，只是每调用一次就往该列表中增加了一个元素。


## Mac上如何将pip可执行文件放入全局

```python
sudo cp /Library/Frameworks/Python.framework/Versions/3.6/bin/virtualenv /usr/local/bin/
```
参考文章：
- [Mac上command not found: virtualenv](https://blog.csdn.net/weixin_42539198/article/details/88739542)

## SVN: E155021: This client is too old to work...
SVN版本太旧了，只需要设置一下svn内置的版本就可以了
![snailsvnlite](https://tva1.sinaimg.cn/large/006y8mN6ly1g6pkh7xs01j30qi0kcn0l.jpg)


## 避免环形引用

```python
# 层级结构

├── script.py
├── tasks
│   ├── login.py
│   └── user.py
```
```python
# script.py

from tasks.user import User
```
```python
# user.py

from tasks.login import Login

class User:
    pass
```
```python
# login.py

from tasks.user import User

class Login:
    pass
```
1. 首先是script.py导入了user.User；
2. 而在user.py中，又导入了login.Login；
3. 在login.py中，又导入了user.User；
4. user.py和login.py形成了循环引用，导致模块导入失败

解决方案：

1. 只导入包名，不具体导入，而是使用`.方法`的形式。eg: import user
2. 延迟导入。在文件中间、结尾或者函数内部导入
3. 调整代码层级设计

参考文章：
- [关于python中模块的环状引用(circular imports)](https://blog.csdn.net/yuanliang01xiaolang/article/details/50529871)
- [python解决循环引用问题](https://www.jianshu.com/p/a1e91cc53b07)
- [python模块循环引用导致问题](https://www.imooc.com/article/34235)

## Centos环境初始化

```pyhton
# 编译时会出现缺失“Python.h”错误

# 需要安装python-dev包，该包在ubuntu和yum上的名称不同

# 在centos上：
yum install python-devel  # 安装python2-dev
yum install python36-devel  # 安装python3.6-dev

# 在ubuntu上：（未测试）
yum install python-dev  # 安装python2-dev
yum install python36-dev  # 安装python3.6-dev

```

## Python 内存泄漏
内存泄漏指的是：对象创建时分配内存，但结束时该对象的内存没有被释放（存在引用）。这种情况导致了随着程序的运行，会出现越来越多对象被创建却没有释放的情况，导致内存占用越来越大。

两种场景：

1. 程序不需要长时间运行，可以不用考虑内存泄漏。Python程序退出会自动删除所有的对象。
2. 程序需要长时间运行，必须考虑内存泄漏。

出现内存泄漏的原因：
1. 应该被回收的对象被生命周期更长的对象所引用。如scrapy中，Request是生命周期最长的对象。假如将Response对象通过meta与Request对象绑定，同时Response的最大存活时间比Request的最大存活时间还要长，则一定是发生了内存泄漏。
2. 存在多重引用。

```python
class DemoClass:
    def __del__(self):
        print("DemoClass对象内存占用被回收...")

d1 = DemoClass()
d2 = d1
del d1  # 本应该在这里打印del的内容
# 但却在这里打印了。

出现这种情况的原因是：d2引用了d1，导致DemoClass一共出现了两个引用(d1/d2)。
因此，仅仅删除d1不会触发垃圾回收机制，因而就不会执行del中的内容
在程序最后退出的时候，会回收掉所有的对象，因此这里会执行DemoClass的del方法
```
3. 存在循环引用或者交叉引用。
参考文章：

- [Scrapy的内存泄露问题总结](https://www.imooc.com/article/50843)


## property 动态属性
```python
class User1:
    """property 动态属性"""

    def __init__(self, name):
        self.name = name

    @property
    def age(self):
        return 18
```

## 属性描述符与非属性描述符
```python
class IntField(object):
    """数据属性描述符"""

    def __get__(self, instance, owner):
        pass

    def __set__(self, instance, value):
        assert isinstance(value, int), "value must is a int"

    def __delete__(self, instance):
        pass


class NonDataIntField(object):
    """非数据属性描述符"""

    def __get__(self, instance, owner):
        return self.value


class User2:
    age = NonDataIntField()
```
