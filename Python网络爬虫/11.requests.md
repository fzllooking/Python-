# 基本使用
```PYTHON
1.文档:
官方文档
http://cn.python-requests.org/zh_CN/latest/
快速上手
http://cn.python-requests.org/zh_CN/latest/user/quickstart.html
```

```PYTHON
2.安装
pip install requests
```
```python
3.response的属性以及类型
```
|类型|models.Response|
|-------|-------|
|r.text|类型为字符串，获取网站源码|
|r.encoding|类型为字符串，用于访问或定制编码方式。|
|r.url|类型为字符串，获取请求的 URL|
|r.content|类型为字节，响应的字节类型|
|r.status_code|类型为整数，响应的状态码|
|r.headers|类型为字典，响应的头信息|

* response.text()
```PYTHON
import requests
url = 'http://www.baidu.com'
response = requests.get(url=url)

print(response.text)
# 返回了网站源码数据,以字符串形式
```
* response.encoding
```PYTHON
response.encoding = 'utf-8'
# 设置爬取数据编码
```
* response.url
```PYTHON
print(response.url)
# http://www.baidu.com/
```
* response.content
```python
print(response.content)
# 返回网站源代码数据,以二进制b的形式
```
* response.status_code
```PYTHON
# 返回相应的状态码
print(response.status_code)
# 200
```
* response.headers
```python
# 返回响应头
print(response.headers)
```
