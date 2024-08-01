# requests.post()
* post请求不需要编解码
* post请求的参数是data
* 不需要请求对象的定制
```PYTHON
import requests
import json
url = 'https://fanyi.baidu.com/sug'
headers={
    'User-Agent': '......'
}

data = {
    'kw':'研究'
}
# kwargs: 字典
response = requests.post(url=url,data=data,headers=headers)
content = response.text
# print(content)

obj = json.loads(content)
print(obj)

# rno': 0, 'data': [{'k': '研究', 'v': 'research; study; consider; discuss'}, {'k': '研究会', 'v': 'seminar; [经] board'}, {'k': '研究出', 'v': 'excogitate'}, {'k': '研究员', 'v': 'researcher ; boffin'}, {'k': '研究型', 'v': 'research-based'}], 'logid': 924647986}
```
