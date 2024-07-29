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
