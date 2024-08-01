# BeautifulSoup
```PYTHON
1.Beautifulsoup简称:
  bs4
2.什么是Beatifulsoup?
  Beautifulsoup，和lxml一样，是一个html的解析器,主要功能也是解析和提取数据
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
#### (1)find()--返回一个对象
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
* 根据class属性查找
* 因为class是python中的关键字,因此bs4规定使用class_代替
```python
print(soup.find('a',class_="a1"))
```

#### (2)find_all()--返回一个列表
> find_all(name, attrs, recursive, string, limit, **kwargs)

> name：可以是标签的名称，如 'div'、'p' 等。
例如：soup.find_all('div') 会找到所有的 <div> 标签。

> attrs：是一个字典，用于指定标签的属性及对应的值。

> 比如：soup.find_all(attrs={'class': 'example'}) 会找到所有 class 属性为 example 的标签。

> recursive：是一个布尔值，决定是否递归搜索子标签。默认值为 True。

> string：用于基于标签中的字符串内容进行搜索。

> limit：限制返回的结果数量。

> 
```PYTHON
print(soup.find_all('a'))

# 返回的是列表: [<a href="">这是a标签</a>, <a href="" title="a2">这是a_22标签</a>]
```

```PYTHON
print(soup.find_all('a',class_="a1"))

# [<a class="a1" href="" id="12">这是a标签</a>]

```
* 同时获取多个标签--find_all()中要用列表类型的数据
```python
print(soup.find_all(['a','span']))

# [<a href="">这是a标签</a>, <span>hhh</span>, <a href="" title="a2">这是a_22标签</a>]

# limit--查找前几个数据
print(soup.find_all('li',limit=2))

# [<li class="c1" id="l1">北京</li>, <li>上海</li>]
```
#### (3)select() -- 返回列表，并且返回多个数据
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

* 获取节点内容,适用于标签中嵌套标签的结构
  * obj.string()
  * obj.get_text()
```PYTHON
obj = soup.select('#d1')[0]
# 如果标签对象中只有内容，那么string和get_text都可以使用
# 标签中含了标签，那么obj.string()就不可以显示其中的数据
# 如果标签对象中除了内容还有标签，只能用get_text()
print(obj.string)
print(obj.get_text())
```
* 现增添一道信息
```html
<p id = "p1" class="p1">呵呵呵你好啊</p>>
```
* 节点的属性
```PYTHON
obj =soup.select('#p1')[0]
# name是标签的名字
print(obj.name)

# 将属性值作为字典返回:attrs
print(obj.attrs)

# 获取节点的属性
obj = soup.select('#p1')[0]
# 推荐使用第一种
print(obj.attrs.get('class')) 
print(obj.get('class'))
print(obj['class'])
```

## BeautifulSoup实战
* 爬取小说信息
![image](https://github.com/user-attachments/assets/5efdd4b5-1014-45fc-98cc-286ae7f8bca1)

```PYTHON
from bs4 import BeautifulSoup
import urllib.request

url = 'https://7haojidi.com/book/62591/23282368.html'
headers = {
    'user-agent':'......'

request = urllib.request.Request(url=url, headers=headers)
response = urllib.request.urlopen(request)
content = response.read().decode('utf-8')

soup = BeautifulSoup(content, 'lxml')

# //p/text() 找到了小说文字对应的标签
novel_list = soup.select('p')

for novel in novel_list:
    print(novel.get_text()) 

# 结果为：“你们主人是谁？”白宇哲虽然心中有所猜测，不过还是开口问了一句。
 “这就不需要你多问了，直接跟我们走就是！”那男子言简意赅，就跟命令一般，容不得白宇哲有半点反驳，甚至一边说着话，一边就已经将白宇哲围在了中间。
 恐怕只要白宇哲说出半个不字，他们就会立刻动手，强行将白宇哲带走。
 “呵呵，看你们的样子就知道你们的主人也就这么回事，毕竟养的狗就已经暴露了他本性！”见对方如此态度，白宇哲自然也不会给什么好脸色看，对方说不说他其实都已经猜到了。
 白宇哲的话一出口，对方几人顿时脸色剧变，四股杀气瞬间锁定了白宇哲。他们虽然只是下人，但是在外面，却都是高人一等的存在，在浮城基本没人敢得罪，因为他们的主子地位足够高，随便跺跺脚都能让浮城晃三晃。
 “敬酒不吃吃罚酒！”那男子一声冷哼，哐当一声长剑出鞘，直接一剑刺向了白宇哲的胸口！虽然主子说要带去见他，没说要直接杀了，但是就凭对方刚才那句话，他就忍不下这口气，弄个半死再带过去也无所谓。
 剑光闪烁，源力包裹着剑身，寒气逼人，速度极快，瞬间就到了白宇哲的胸前，对方赫然是灵窍境的高手！
......
```

* 下载到本地
```PYTHON
for novel in novel_list:
    print(novel.get_text())
    with open('novel_1.txt','a',encoding='utf-8')as fp:
        fp.write(novel.get_text())
```
![image](https://github.com/user-attachments/assets/a4062c43-2394-4b16-8934-e09652c8368e)
