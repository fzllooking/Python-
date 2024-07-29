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
