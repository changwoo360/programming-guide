## 导航

- [programming-guide](http://39.105.52.60:60001/)
- [Debugs](https://www.notion.so/0ccf5aa46809446ea39a1088106efd25)
- [Github](https://github.com/xiaoyinyulin/SimpleWisdom)
- [博客园](https://www.cnblogs.com/changwoo/)


## 代码规范
参考文章：
1. [Google 开源项目风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)
2. [PEP8中文版 -- Python编码风格指南](https://python.freelycode.com/contribution/detail/47)
3. [常见编程命名缩写](https://blog.csdn.net/wf19930209/article/details/78577918)
4. [如何写出无法维护的代码](https://coolshell.cn/articles/4758.html)
5. [列表推导式与表达式生成器在 Python 中的滥用](https://juejin.im/post/5d281b0ff265da1b8b2b8ae0)
6. [Python 工匠：善用变量来改善代码质量](https://juejin.im/post/5acc5b975188255c61635335)
7. [Python 工匠：编写条件分支代码的技巧](https://juejin.im/post/5acd4335518825558c47c558)

### 命名
> 计算机科学只存在两个难题：缓存失效和命名。

```python
# 变量
name = "bob"
names = ["bob", "alex"]

# 常量
INSTALLED_APPS = ["app1", "app2"]

# 函数
def parse_html(): pass

# 类
class ProxyFilter: pass

# 临时使用的变量(暂未好的解决方案)
_name = "bob ".strip()
name = "Jin" + _name
```

### 注释
#### 声明
```python
#! /usr/bin/env python3
# -*- coding:utf-8 -*-
```
#### 函数注释
```python
def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
    """Fetches rows from a Bigtable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by big_table.

    Args:
        big_table: An open Bigtable Table instance.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. 

    Raises:
        IOError: An error occurred accessing the bigtable.Table object.
    """
    pass
```

#### 类注释
```python
class SampleClass(object):
    """Summary of class here.

    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam=False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def public_method(self):
        """Performs operation blah."""
```

### 优秀代码展示
```python
def get_factors(dividend):
    """返回所给数值的所有因子作为一个列表。"""
    return [
        n
        for n in range(1, dividend+1)
        if dividend % n == 0
    ]
```
```text
is_superuser：『是否超级用户』，只会有两种值：是/不是
has_error：『有没有错误』，只会有两种值：有/没有
allow_vip：『是否允许 VIP』，只会有两种值：允许/不允许
use_msgpack：『是否使用 msgpack』，只会有两种值：使用/不使用
debug：『是否开启调试模式』，被当做 bool 主要是因为约定俗成
```
```python
def get_best_trip_by_user_id(user_id):
    """减少了两个变量的命名"""
    return {
        'user': get_user(user_id),
        'trip': get_best_trip(user_id)
    }
```
```python
# 不必预计算字面量表达式

delta_sencods = 11 * 24 * 3600
```
```python
# 使用括号隐式换行

logger.info(("There is something really bad happened during the process. "
             "Please contact your administrator."))
```
```python

```


### 心得
1. 命名规范优先级：公司命名规范 > pep8规范 > 惯用规范
2. 对于废弃的代码，应删除它，而不是注释。假如你很快要用到，就使用注释。
3. 注释的内容尽量是功能介绍+原因
4. TODO注释要在提交代码前删除掉
5. 如果存在很深的嵌套，提早使用return
6. 不要滥用匈牙利命名法，只有在实在起不了名字再考虑使用


### 开发经验

1. 熟悉所在团队的成员，并了解各自的技术栈。
2. 要锻炼自己的沟通表达能力。首要一点，要让对象了解到你的需求。
3. 及时重构，还清技术债务。越早重构成本越小。
4. 如果有结对编程或者code review的机会，一定要去尝试。这是一次很难得的提升机会。
5. 不要害怕程序被发现bug。bug越早被发现，危害越小。