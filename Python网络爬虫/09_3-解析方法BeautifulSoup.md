# BeautifulSoup
```PYTHON
1.Beautifulsoup简称:
  bs4
2.什么是Beatifulsoup?
  Beautifulsoup，和lxm1一样，是一个html的解析器,主要功能也是解析和提取数据
3.优缺点?
  缺点:效率没有lxml的效率高
  优点:接口设计人性化，使用方便
```
* BeautifulSoup安装
```PYTHON
1.安装
  pip install bs4
2.导入
  from bs4 import Beautifulsoup
3.创建对象
  3_1.服务器响应的文件生成对象
    soup =Beautifulsoup(response.read().decode()，'lxml')
  3_2.本地文件生成对象
    soup = Beautifulsoup(open('1.html'),'lxml')
    注意:默认打开文件的编码格式gbk所以需要指定打开编码格式
```
* 现有一个html文件：
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
<!--utf-8后面要加/，代表结束-->
    <meta charset="UTF-8"/>
    <title>Title</title>
</head>
<body>

    <div>

    <ul>
        <li id="l1" class="c1">北京</li>
        <li>上海</li>
        <li>深圳</li>
        <li>武汉</li>
        <a href="" ID="12" class="a1">这是a标签</a>
        <span>hhh</span>
    </ul>
    </div>
        <a href=""  title = "a2">这是a_22标签</a>
</body>
</html>
```

## bs4语法举例：
### 节点定位
* 首先初始化：
```PYTHON
from bs4 import BeautifulSoup
* 解析本地文件
soup = BeautifulSoup(open('xpath.html',encoding='utf-8'),'lxml')
# print(soup) 获取了源文件
```

* 1.根据标签名查找数据--找到的是第一个符合的数据
```PYtHON
print(soup.a)
# <a href="">这是a标签</a>
```

* 2.soup.a.attrs获取标签的属性和属性值
```python
print(soup.a.attrs)
# {'href': ''}
```
### bs4函数
* (1)find()--返回一个对象
```PYTHON
# find()函数--返回的是第一个数据
print(soup.find('a'))

# <a href="">这是a标签</a>  
```
* 根据title属性找到相应标签
```python
print(soup.find('a',title='a2'))

# <a href="" title="a2">这是a_22标签</a>
```
* (2)find_all()--返回一个列表
```PYTHON
print(soup.find_all('a'))

# 返回的是列表: [<a href="">这是a标签</a>, <a href="" title="a2">这是a_22标签</a>]
```
* 同时获取多个标签--find_all()中要用列表类型的数据
```python
print(soup.find_all(['a','span']))

# [<a href="">这是a标签</a>, <span>hhh</span>, <a href="" title="a2">这是a_22标签</a>]

# limit--查找前几个数据
print(soup.find_all('li',limit=2))

# [<li class="c1" id="l1">北京</li>, <li>上海</li>]
```
* (3)select() -- 返回列表，并且返回多个数据
```PYTHON
print(soup.select('a'))

# [<a href="">这是a标签</a>, <a href="" title="a2">这是a_22标签</a>]
```
* 类选择器--> class属性转换成了.
```python
print(soup.select('.a1'))

# [<a class="a1" href="" id="">这是a标签</a>]
```
* id属性--> #
```PYTHON
print(soup.select('#l1'))

# [<li class="c1" id="l1">北京</li>]
```
* 属性选择器
* 查找li标签中有id的标签
```PYTHON
print(soup.select('li[id]'))
# 查找li标签中id为l1的属性
print(soup.select('li[id="l1"]'))

# [<li class="c1" id="l1">北京</li>]
```

* 层级选择器 注意；div后面要加“空格”
** 后代选择器
```PYTHON
# 找到div下的li
print(soup.select('div li'))

# <li class="c1" id="l1">北京</li>, <li>上海</li>, <li>深圳</li>, <li>武汉</li>]
```
* 子代选择器--某标签的第一级子标签
```python
print(soup.select('div >ul >li'))

# [<li class="c1" id="l1">北京</li>, <li>上海</li>, <li>深圳</li>, <li>武汉</li>]

# 找到a标签和li标签的所有对象--和find_all()不同，不用加列表
print(soup.select('a,li'))

# [<li class="c1" id="l1">北京</li>, <li>上海</li>, <li>深圳</li>, <li>武汉</li>, <a class="a1" href="" id="12">这是a标签</a>, <a href="" title="a2">这是a_22标签</a>]
```

### 节点信息
* 现在添加一道div信息：
```HTML
<div id="d1">
        <span>
         哈哈哈
        </span>
    </div>>
```
