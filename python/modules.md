### re —— 正则匹配

循环多次匹配规则
```python
re.match(r"(\d{4}(?:[^0-9]?\d{2}){5})", dt)

# 2020-01-08 15:26:26.23234 --> 2020-01-08 15:26:26
# (?:asd)+ 是正则的一种不存分组的语法, 它可以将`asd`看成一个样式整体, 所以当我们用+时, 就能代表多个asd
```




### bypy —— 操作百度网盘
一、基础配置
安装
```bash shell
pip3 install bypy
```
认证
```bash shell
# 需要复制网址到浏览器进行授权认证
bypy info
```

二、基本使用
```bash shell
# 上传test.md
bypy upload test.md

# 下载test.md
bypy downfile test.md

# 上传当前文件夹下所有内容（进入到当前路径）
bypy upload

# 下载云盘上的所有文件
bypy downdir /

# 下载云盘上执行目录的文件
bypy downdir /test/test.md

# 查看根目录下的文件
bypy list

# 查看指定目录的文件
bypy list test/

# 退出登录
bypy -c
```

#### 参考文章
- [bypy安装和使用](https://blog.csdn.net/qq_35425070/article/details/96577512)
- [bypy-github](https://github.com/houtianze/bypy)
- []
