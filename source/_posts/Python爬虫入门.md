---
title: Python爬虫入门
date: 2021-07-17 00:59:06
tags:
---

# 0x01 入门

requests是一款基于网络请求的模块，用来模拟浏览器发送请求



requests模块的编码流程：

- 指定URL
- 发起请求
- 获取响应数据
- 持久化存储



环境安装：

```python
pip install requests
```



实战编码：

- 指定需求：爬取搜狗首页的页面数据

```python
import requests

url='https://www.sogou.com/' #指定URL
resp=requests.get(url=url) #发起请求
page_text=resp.text #获取响应数据,返回的是字符串类型的响应数据，网站源码
with open('./sogou.html','w',encoding='utf-8') as fp: #持久化存储
    fp.write(page_text)
print('爬取数据结束')
```





实战巩固：

- 爬取搜狗指定词条对应的搜索结果页面（简单网页采集器）
- 爬取百度翻译
- 爬取豆瓣电影分类排行榜中的电影详情数据
- 爬取肯德基餐厅查询中指定地点的餐厅数量
- 爬取国家药品监督总局中基于中华人民共和国化妆品生产许可证相关数据



# 0x02 爬取搜狗指定词条对应的搜索结果页面（简单网页采集器）

```python
import requests

url='https://www.sogou.com/web'
kw=input("输入关键字")
headers={
    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SPN-AL00 Build/HUAWEISPN-AL00; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/77.0.3865.120 MQQBrowser/6.2 TBS/045709 Mobile Safari/537.36 MMWEBID/7142 MicroMessenger/8.0.7.1920(0x28000737) Process/tools WeChat/arm64 Weixin NetType/WIFI Language/zh_CN ABI/arm64'
}
param={
    'query':kw
}
resp=requests.get(url=url,params=param,headers=headers)
page_text=resp.text
filename=kw +'.html'
with open(filename,'w',encoding='utf-8') as fp:
    fp.write(page_text)
```







# 0x03 百度翻译

```python
import requests
import json
url='https://fanyi.baidu.com/sug'
kw=input("请输入关键字")
headers={
    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SPN-AL00 Build/HUAWEISPN-AL00; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/77.0.3865.120 MQQBrowser/6.2 TBS/045709 Mobile Safari/537.36 MMWEBID/7142 MicroMessenger/8.0.7.1920(0x28000737) Process/tools WeChat/arm64 Weixin NetType/WIFI Language/zh_CN ABI/arm64'
}
data={
    'kw':kw
}
resp=requests.post(url=url,data=data,headers=headers)
page_data=resp.json()
fp=open('/dog.json','w',encoding='utf-8')
json.dump(page_data,fp=fp,ensure_ascii=False)
print("结束")
```





# 0x04 爬取豆瓣电影分类排行榜中的电影详情数据

 ```python
import requests
import json
url='https://movie.douban.com/j/chart/top_list'
headers={
    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SPN-AL00 Build/HUAWEISPN-AL00; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/77.0.3865.120 MQQBrowser/6.2 TBS/045709 Mobile Safari/537.36 MMWEBID/7142 MicroMessenger/8.0.7.1920(0x28000737) Process/tools WeChat/arm64 Weixin NetType/WIFI Language/zh_CN ABI/arm64'
}
params={
    'type':'20',
    'interval_id':'100:90',
    'action':'',
    'start':'20',
    'limit':'20'
}
resp=requests.get(url=url,headers=headers,params=params)
list_data=resp.json()
fp=open('D:\\doban.json','w',encoding='utf-8')
json.dump(list_data,fp=fp,ensure_ascii=False)
print("结束")
 ```







# 0x05 爬取肯德基餐厅查询中指定地点的餐厅数量

```python
import requests
word=input("请输入要查询的地址：")
url='http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=keyword'
headers={
    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SPN-AL00 Build/HUAWEISPN-AL00; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/77.0.3865.120 MQQBrowser/6.2 TBS/045709 Mobile Safari/537.36 MMWEBID/7142 MicroMessenger/8.0.7.1920(0x28000737) Process/tools WeChat/arm64 Weixin NetType/WIFI Language/zh_CN ABI/arm64'
}
data={
    'cname':'',
    'pid':'',
    'keyword':word,
    'pageIndex':'1',
    'pageSize':'10'
}
resp=requests.post(url=url,data=data,headers=headers)
page_text=resp.text()
with open('D:\\a.xml','w',encoding='utf-8') as fp:
    fp.write(page_text)

```







# 0x06 爬取国家药品监督总局中基于中华人民共和国化妆品生产许可证相关数据

```python
import requests
import json
url='http://scxk.nmpa.gov.cn:81/xk/itownet/portalAction.do?method=getXkzsList'
headers={
    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SPN-AL00 Build/HUAWEISPN-AL00; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/77.0.3865.120 MQQBrowser/6.2 TBS/045709 Mobile Safari/537.36 MMWEBID/7142 MicroMessenger/8.0.7.1920(0x28000737) Process/tools WeChat/arm64 Weixin NetType/WIFI Language/zh_CN ABI/arm64'
}
data={
    'on': 'true',
    'page':'1',
    'pageSize':'15',
    'productName':'',
    'conditionType':'1',
    'applyname':'',
    'applysn':''
}
resp=requests.post(url=url,headers=headers,data=data).json()

fli=[]
for dic in resp['list']:
    fli.append(dic["ID"])

url1='http://scxk.nmpa.gov.cn:81/xk/itownet/portalAction.do?method=getXkzsById'
for id in fli:
    data1={
        'id':id
    }
    response=requests.post(url=url1,headers=headers,data=data1).json()
    fp = open('D:\\do.json', 'w', encoding='utf-8')
    json.dump(response, fp=fp, ensure_ascii=False)
print("结束")

```

   



# 0x07 图片的爬取

先爬取整张页面，再解析局部

```python
import requests
url='https://pic.imgdb.cn/item/60ec65435132923bf8dfeea6.png'
image_data=requests.get(url=url).content
with open('D:\\1.jpg','wb') as fp:
    fp.write(image_data)
```





# 0x08 正则案例1

常用正则表达式：

```shell
单字符：	.:除了换行以外所有字符	[]:[aoe] [a-w]匹配集合中任意一个字符   	\d:数字[0-9]	\D:非数字	\w:数字、字母、下划线、中文	\W:非\w	\s:左右的空白字符，包括空格、制表符、换页符等等，等价于[\f\n\r\t\v]	\S:非空白	数量修饰：	*:任意多次 >=0	+:至少一次 >=1	?:可有可无 0次或1次	{m}:固定m次 hello{3,}	{m,}:至少m次	{m,n}:m-n次	边界：	$:以某某结尾	^:以某某开头分组：	(ab)贪婪模式：	.*非贪婪模式：	.*?	re.I：忽略大小写re.M：多行匹配re.S：单行匹配re.sub(正则表达式,替换内容,字符串)
```



正则练习：

```python
import re #提取出pythonkey='javapythonc++php' re.findall('python',key)[0]#提取出hello worldkey="<html><h1>hello world<h1></html>"re.findall('<h1>(.*)',key)[0]#提取出170string='我喜欢身高为170的女孩're.findall('\d+',string)#提取出http:// 和 https://key='http://www.baidu.com and https://boob.com're.findall('https?://',key)
```

