

反转列表
```python
li = [1, 2, 3]
li[::-1]
```

合并两个字典
```python
dt_1, dt_2 = {"a1": 1, "b1": 2}, {"a2", 1, "b2": 2}
dict(dt_1, **dt_2)
```

判断元素都为真
```python
all([1, 2, 3])
```

判断至少一个元素为真
```python
any([1, 2, 3])
```

进制转换
```python
bin(10)  # 十进制 --> 二进制
oct(10)  # 十进制 --> 八进制
hex(10)  # 十进制 --> 十六进制

chr(10)  # 十进制 --> ASCII字符
ord("A")  # ASCII字符 -- >十进制
```

字符串转字节
```python
bytes("str", encoding="utf8")
```

执行字符串代码
```python
s = "print('华电集团电子商务平台')"
obj = compile(s, "", "exec")
exec(obj)
```


