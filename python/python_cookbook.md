### 字符串与文本
使用多个界定符分割字符串
```python
import re

line = 'asdf fjdk; afed, fjek,asdf, foo'
res = re.split("[;,\s]\s*", line)

>>> ['asdf', 'fjdk', 'afed', 'fjek', 'asdf', 'foo']
```

字符串匹配开头或者结尾
```python
if name.endswith((".py", ".pyc", ".md")): pass

if any(name.endswith((".py", ".pyc")) for name in listdir(dirname)): pass
```



