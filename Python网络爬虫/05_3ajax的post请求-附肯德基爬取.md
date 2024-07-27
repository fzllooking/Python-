> 例如，对肯德基页面进行爬取数据(post请求)
![QQ_1722071368025](https://github.com/user-attachments/assets/b6570d5d-42f1-4898-b8b5-b1b5b0ba2284)

* 相应的data数据如下：
![QQ_1722071427382](https://github.com/user-attachments/assets/a9f07a04-c221-496d-821e-f82fb13f1226)

```PYTHON
#第一页 http://ww.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=cname
# post
# cname:北京
# pid:
# pageIndex:1
# pageSize: 10
# 第二页: http://ww.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=cname

import urllib.request
import urllib.parse


def create_request(page):
    base_url = 'http://ww.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=cname'
    data={
        'cname':'北京',
        'pid':'',
        'pageIndex':page,
        'pageSize':'10'
    }
    data = urllib.parse.urlencode(data).encode('utf-8')
    headers = {
        'User-Agent': '......'
    }

    request = urllib.request.Request(url=base_url,headers=headers,data=data)

def get_content(request):
    response = urllib.request.urlopen(request)
    content = response.read().decode('utf-8')

def down_load(page,content):
    with open('kfc'+ str(page)+'.json','w',encoding='utf-8')as fp:
        fp.write()

if __name__ == '__main__':
    start_page = int(input('请输入起始页码'))
    end_page = int(input('请输入结束页码'))

    for page in range(start_page, end_page+1):
        print(page)
# 每一页都有请求对象的定制
        request = create_request(page)
# 获取相应的数据
        content = get_content(request)
# 下载
        down_load(page,content)
```
