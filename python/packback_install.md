```shell script
# ModuleNotFoundError: No module named 'Crypto'
pip install pycryptodome

# ModuleNotFoundError: No module named 'win32con'
pip install pypiwin32

# ModuleNotFoundError: No module named 'rsa'
pip install -i https://pypi.douban.com/simple pyasn1==0.1.3
pip install -i https://pypi.douban.com/simple rsa

# ModuleNotFoundError: No module named 'Crypto'
pip install pycryptodome

# ModuleNotFoundError: No module named 'win32con'
pip install pypiwin32

# ModuleNotFoundError: No module named 'rsa'
pip install -i https://pypi.douban.com/simple pyasn1==0.1.3
pip install -i https://pypi.douban.com/simple rsa
```

```shell script

j， s: 向下滚动
k， w: 向上滑动
h: 向左滚动
l: 向右滚动
d: 滚动一半页面
未映射: 滚动全页下来
u， e: 滚动一半页面
未映射: 滚动全页面
gg: 滚动到页面顶部
G: 滚动到页面的底部
0: 滚动到页面的左边
$: 滚动到页面的右侧
#: 将滚动焦点重置为主页面
gi: 转到第一个输入框
gI: 转到最后一个聚焦的输入框 gi
zz: 中心页到当前搜索匹配（中）
zt: 中心页到当前搜索匹配（上）
zb: 中心页面到当前搜索匹配（底部）
链接提示
f: 打开当前选项卡中的链接
F: 在新标签页中打开链接
未映射: 在新标签页中打开链接（活动）
W: 在新窗口中打开链接
A: 重复最后提示命令
q: 触发悬停事件（mouseover + mouseenter）
Q: 触发不起眼的事件（mouseout + mouseleave）
mf: 打开多个链接
未映射: 使用外部编辑器编辑文本
未映射: 调用带有链接的代码块作为第一个参数
未映射: 在新标签页中打开图像
mr: 反向图像搜索多个链接
my: y多个链接（打开与P的链接列表）
gy: 从链接复制URL到剪贴板
gr: 反向图像搜索（谷歌图像）
;: 改变链接提示的重点
QuickMarks
M<*>: 创建quickmark <*>
go<*>: 在当前标签中打开quickmark <*>
gn<*>: 在新标签页中打开quickmark <*>
gw<*>: 在新窗口中打开quickmark <*>
杂
a: 别名为“：tabnew google”
.: 重复最后一个命令
:: 打开命令栏
/: 打开搜索栏
?: 打开搜索栏（反向搜索）
未映射: 打开链接搜索栏（与按下相同/?）
I: 搜索浏览器历史记录
<N>g%: 在页面上滚动<N>百分比
<N>未映射: 将<N>密钥传递到当前页面
zr: 重新启动Google Chrome
i: 进入插入模式（退出退出）
r: 重新加载当前选项卡
gR: 重新加载当前选项卡+本地缓存
;<*>: 创建标记<*>
'': 转到最后滚动位置
<C-o>: 转到上一个滚动位置
<C-i>: 转到下一个滚动位置
'<*>: 去标记<*>
cm: 静音/取消静音选项卡
没有: 重新加载所有选项卡
cr: 重新加载所有选项卡，但当前
zi: 缩放页面
zo: 缩小页面
z0: 缩放页面到原始大小
z<Enter>: 切换图像缩放（与仅在图像页面上单击图像相同）
gd: 别名：chrome：//下载<CR>
ge: 别名：chrome：// extensions <CR>
yy: 将当前页面的URL复制到剪贴板
yY: 将当前帧的URL复制到剪贴板
ya: 复制当前窗口中的URL
yh: 从查找模式复制当前匹配的文本（如果有）
b: 搜寻书签
p: 打开剪贴板选择
P: 在新标签页中打开剪贴板选择
gj: 隐藏下载架
gf: 循环通过iframe
gF: 去根框架
gq: 停止加载当前选项卡
gQ: 停止加载所有标签
gu: 在URL中上传一条路径
gU: 转到基本URL
gs: 转到当前网址的view-source：//页面
<C-b>: 创建或切换当前网址的书签
未映射: 关闭所有浏览器窗口
g-: 减去URL路径中的第一个数字（例如www.example.com/5=> www.example.com/4）
g+: 增加URL路径中的第一个数字
标签导航
gt，K，R: 导航到下一个选项卡
gT，J，E: 导航到上一个选项卡
g0， g$: 转到第一个/最后一个选项卡
<C-S-h>， gh: 在新标签页中打开当前标签的历史记录中的最后一个URL
<C-S-l>， gl: 在新标签页中打开当前选项卡的历史记录中的下一个URL
x: 关闭当前选项卡
gxT: 关闭当前选项卡左侧的选项卡
gxt: 关闭当前选项卡右侧的选项卡
gx0: 关闭当前选项卡左侧的所有选项卡
gx$: 关闭当前选项卡右侧的所有选项卡
X: 打开最后关闭的标签页
t: ：执行tabnew
T: ：tabnew <CURRENT URL>
O: ：打开<CURRENT URL>
<N>%: 切换到选项卡<N>
H， S: 回去
L， D: 前进
B: 搜索另一个活动选项卡
<: 向左移动当前标签
>: 向右移动当前标签
]]: 点击页面上的“下一个”链接（见上面的nextmatchpattern）
[[: 点击页面上的“返回”链接（参见上面的上一个模式）
gp: 引导/取消固定当前选项卡
<C-6>: 在最后使用的标签之间切换焦点
查找模式
n: 下一个搜索结果
N: 以前的搜索结果
v: 进入视觉/插入模式（突出显示当前搜索/选择）
V: 从插入模式/当前突出显示的搜索进入视线模式
未映射: 清除搜索模式突出显示
视觉/插入模式
<Esc>: 将视觉模式退出插入模式/退出插入模式到正常模式
v: 在视觉/插入模式之间切换
h，j，k，l: 移动插入位置/扩展视觉选择
y: 配合当前的选择
n: 选择下一个搜索结果
N: 选择先前的搜索结果
p: 打开当前选项卡中的突出显示
P: 在新标签中打开突出显示的文本
文本框
<C-i>: 将光标移动到行的开头
<C-e>: 将光标移动到行尾
<C-u>: 删除到行的开头
<C-o>: 删除到行尾
<C-y>: 删除一个字
<C-p>: 删除一个字
未映射: 删除一个字符
未映射: 删除一个字符
<C-h>: 将光标移回一个字
<C-l>: 将光标向前移动一个字
<C-f>: 将光标向前移动一个字母
<C-b>: 将光标移回一个字母
<C-j>: 将光标向前移动一行
<C-k>: 将光标移回一行
未映射: 选择输入文本（相当于<C-a>）
未映射: 在终端中使用Vim进行编辑（需要运行该工作的cvim_server.py脚本以及该脚本中的VIM_COMMAND集）

```