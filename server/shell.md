

### 获取python文件的返回值
```bash shell
word=`python3 test.py`
echo $word
```
### 正则匹配
```bash shell
str_sub=`echo $compress_path | grep -Po '附件路径（(.*?)）'`

```
