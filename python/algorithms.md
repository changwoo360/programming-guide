
## 排序算法
### 冒泡排序

```python
def bubble_sort(li):
    length = len(li)
    is_sorted = True
    for index in range(length - 1):
        if li[index] > li[index + 1]:
            li[index], li[index + 1] = li[index + 1], li[index]
            is_sorted = False

    if not is_sorted:
        bubble_sort(li)

    return li
```

### 快速排序
```python

```

### 堆排序
```python

```

### 插入排序
```python

```

### 选择性排序
```python

```

### 归并排序
```python

```

