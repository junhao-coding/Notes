#Python网络爬虫
##Requests库

###requests.get()方法
基本使用：**r = requests.get(url)**
说明：通过给定url调用get()方法，在requests内部生成了一个Requests对象，并返回一个包含服务器资源的Response对象。
###Response对象
|属性|说明|
|--|--|
|r.status_code|HTTP请求的返回状态，200表示连接成功，404表示失败|
|r.text |HTTP响应内容的字符串形式，即，url对应的页面内容|
|r.encoding|从HTTP header中猜测的响应内容编码方式|
|r.apparent_encoding|从内容中分析出的响应内容编码方式（备选编码方式）|
|r.content|HTTP响应内容的二进制形式|
###Requests库异常
|异常|说明|
|--|--|
|requests.ConnectionError|网络连接错误异常，如DNS查询失败、拒绝连接等|
|requests.HTTPError|HTTP错误异常|
|requests.URLRequired|URL缺失异常|
|requests.TooManyRedirects|超过最大重定向次数，产生重定向异常|
|requests.ConnectTimeout|连接远程服务器超时异常|
|requests.Timeout |请求URL超时，产生超时异常|
###访问控制参数
* params : 字典或字节序列，作为参数增加到url中
* data : 字典、字节序列或文件对象，作为Request的内容
* json : JSON格式的数据，作为Request的内容
* headers : 字典，HTTP定制头
* cookies : 字典或CookieJar，Request中的cookie
* auth : 元组，支持HTTP认证功能
###实战项目
1. 爬取京东首页的内容
~~~Python
import requests

url='https://www.jd.com/'
try:
    r = requests.get(url)
    r.status_code
    r.encoding = r.apparent_encoding
    print(r.text[:1000])
except:
    print("爬取异常")
~~~
2. 爬取亚马逊某个商品的内容

亚马逊一个商品页面有反爬机制，它通过请求头分析你是通过浏览器发送的HTTP请求还是爬虫，针对爬虫它可以拒绝访问。所以这里我们通过get方法的可选参数定制请求头来模拟用浏览器访问网站。
~~~Python
import requests

url='https://www.amazon.cn/dp/B06Y1MP2PY?ref_=Oct_DLandingS_D_9e9a8281_NA'
try:
    kv={'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.114 Safari/537.36 Edg/103.0.1264.49'}
    r = requests.get(url, headers=kv)
    r.status_code
    r.encoding = r.apparent_encoding
    print(r.text)
except:
    print("爬取异常")
~~~

3.百度搜索关键词提交