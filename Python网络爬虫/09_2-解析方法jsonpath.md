> 对于json数据的爬取,可以使用jsonpath辅助
```python
* jsonpath的安装及使用方式:
* pip安装:
pip install jsonpath
* jsonpath的使用:
obj = json.load(open('json文件','r',encoding='utf-8'))
ret = jsonpath.jsonpath(obj,'jsonpath语法')
# jsonpath只能解析本地的文件，不能解析服务器文件
```
![QQ_1722240995389](https://github.com/user-attachments/assets/5e22e2ff-bbd5-40c0-b043-cf1543f947f9)

* jsonpath使用语法举例
* 以下是一份json数据:
```JSON
{
	"returnCode": "0",
	"returnValue": {
		"A": [{
			"id": 3643,
			"parentId": 0,
			"regionName": "阿坝",
			"cityCode": 513200,
			"pinYin": "ABA"
		}, {
			"id": 3090,
			"parentId": 0,
			"regionName": "阿克苏",
			"cityCode": 652900,
			"pinYin": "AKESU"
		}, {
			"id": 3632,
			"parentId": 0,
			"regionName": "阿拉善",
			"cityCode": 152900,
			"pinYin": "ALASHAN"
		}, {
			"id": 3675,
			"parentId": 0,
			"regionName": "阿勒泰",
			"cityCode": 654300,
			"pinYin": "ALETAI"
		}, {
			"id": 899,
			"parentId": 0,
			"regionName": "安康",
			"cityCode": 610900,
			"pinYin": "ANKANG"
		}, {
			"id": 196,
			"parentId": 0,
			"regionName": "安庆",
			"cityCode": 340800,
			"pinYin": "ANQING",
			"unique": "条件过滤"
		}, {
			"id": 758,
			"parentId": 0,
			"regionName": "鞍山",
			"cityCode": 210300,
			"pinYin": "ANSHAN"
		}, {
			"id": 388,
			"parentId": 0,
			"regionName": "安顺",
			"cityCode": 520400,
			"pinYin": "ANSHUN"
		}, {
			"id": 454,
			"parentId": 0,
			"regionName": "安阳",
			"cityCode": 410500,
			"pinYin": "ANYANG"
		}]
}
}  
```
* 导入基本数据
```PYTHON
import jsonpath
import json

obj = json.load(open('js.json','r',encoding='utf-8'))
```

* 拿到A下面的regionName数据
```python
uthor_list = jsonpath.jsonpath(obj,'$.returnValue.A[*].regionName')
```
* 对于A其下的某一属性求全部：$..
```PYTHON
author_list = jsonpath.jsonpath(obj,'$..regionName')
```
* 拿到A下面的所有数据
```python
author_list = jsonpath.jsonpath(obj,'$..A.*')
```
* 拿到A下面某一个数据
```PYTHON
author_list = jsonpath.jsonpath(obj,'$..A..cityCode')
```
* A下面的第三个城市属性
```PYTHON
author_list = jsonpath.jsonpath(obj,'$..A[2]')
```
* A下面的最后一个城市
```python
author_list = jsonpath.jsonpath(obj,'$..A[(@.length-1)]')
```
* A的前两个城市
```PYTHON
author_list = jsonpath.jsonpath(obj,'$..A[0,1]')
author_list = jsonpath.jsonpath(obj,'$..A[:2]')
```
* 选取A中具有unique属性的城市
```python
author_list = jsonpath.jsonpath(obj,'$..A[?(@.unique)]')
```
* 选中A中id>300的城市信息
```PYTHON
author_list = jsonpath.jsonpath(obj,'$..A[?(@.id>300)]')
```

* 案例：用jsonpath解析淘票票
* 首先从淘票票网站拿取相应数据

![image](https://github.com/user-attachments/assets/9d85b362-440a-41ea-912e-7a69b7cc3be9)

```PYTHON
import urllib.request
url = 'https://dianying.taobao.com/cityAction.json?activityId&_ksTS=1722269482162_108&jsoncallback=jsonp109&action=cityAction&n_s=new&event_submit_doGetAllRegion=true'
headers = {
    # ':authority':'dianying.taobao.com',
    # ':method':'GET',
    # ':path':'/cityAction.json?city=110100&_ksTS=1722268052236_19&jsoncallback=jsonp20&action=cityAction&n_s=new&event_submit_doLocate=true',
    # ':scheme':'https',
    'Accept':'text/javascript, application/javascript, application/ecmascript, application/x-ecmascript, /; q=0.01',
    # 'Accept-Encoding':'gzip, deflate, br, zstd',
    'Accept-Language':'zh-CN,zh;q=0.9',
    'Bx-V':'2.5.14',
    'Cookie':'......',
    'Priority':'u=0, i',
    'Referer': 'https://dianying.taobao.com/',
    'Sec-Ch-Ua':'"Not/A)Brand";v="8", "Chromium";v="126", "Google Chrome";v="126"',
    'Sec-Ch-Ua-Mobile':'?0',
    'Sec-Ch-Ua-Platform':'"Windows"',
    'Sec-Fetch-Dest':'empty',
    'Sec-Fetch-Mode':'cors',
    'Sec-Fetch-Site':'same-origin',
    'User-Agent':'......',
    'X-Requested-With':'XMLHttpRequest'
}

request = urllib.request.Request(url=url,headers=headers)
response = urllib.request.urlopen(request)
content = response.read().decode('utf-8')
print(content)
```
* 结果为：jsonp109({"returnCode":"0","returnValue":{"A":[{"id":3643,"parentId":0,"regionName":"阿坝","cityCode":513200,"pinYin":"ABA"},{"id":3090,"parentId":0,"regionName":"阿克苏","cityCode":652900,"pinYin":"AKESU"},......
* 注意到，这是json格式，因此可以有**split切割**
```PYTHON
#  转换为标准json数据
# split 切割
# content.split('(') -- 将开头的左括号去除切割，形成了['\r\n\r\njsonp109', '{"returnCode":"0","returnValue":{"A":[{"id":3643,"parentId":0,"regionName":"阿坝","......
# content.split('(')[1] -- 取第二个元素， 转换为标准json数据
# content.split('(')[1].split(')') -- 再将最后的)切割去除
# content.split('(')[1].split(')')[0] -- 取第一个元素， 转换为标准json数据

content = content.split('(')[1].split(')')[0]
print(content)

# 结果为：{"returnCode":"0","returnValue":{"A":[{"id":3643,"parentId":0,"regionName":"阿坝",......
```

* 下载到本地后进行jsonpath操作
```PYTHON
with open('城市.json','w',encoding='utf-8')as fp:
    fp.write(content)

# 已经下载到本地后,进行jsonpath操作
import json
import jsonpath
# json.load()里面必须是文件！不能是('城市.json')，因为这个是字符串!
obj = json.load(open('城市.json','r',encoding='utf-8'))
city_list = jsonpath.jsonpath(obj,'$..regionName')
print(city_list)
# ['阿坝', '阿克苏', '阿拉善', '阿勒泰', '安康', '安庆', '鞍山', '......]
```
