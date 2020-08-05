
### 滑动验证码
略

### 滑块验证码
略

### 数字字母验证码
[![an4gyt.png](https://s1.ax1x.com/2020/07/30/an4gyt.png)](https://imgchr.com/i/an4gyt)
```python
from PIL import Image
from pytesseract import image_to_string

img_path = "123.png"
code = image_to_string(Image.open(img_path))  # 27492
```


### 点选验证码
略

### 旋转验证码
略

### 手势验证码
略

### 算术验证码
略
