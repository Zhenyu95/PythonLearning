# Requests

## Basic:

### requests.get

Requests lib general code frame:

```python
import requests
def getHTMLText(url):
    headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
    }
    try:#异常处理
        r=requests.get(url,timeout=30,headers=headers)
        r.raise_for_status()#如果状态不是200，引发HTTPError异常
        r.encoding=r.apparent_encoding
        return r.text
    except:
        return"产生异常"
if__name__=="__main__":
   url="http://www.baidu.com"
   print(getHTMLText(url))
```

### requests.post

```python
import requests
def postHTML(url):
    headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
    }
    formdata = {
    
    }
    try:#异常处理
        r=requests.get(url,timeout=30,headers=headers)
        r.raise_for_status()#如果状态不是200，引发HTTPError异常
        r.encoding=r.apparent_encoding
        return r.text
    except:
        return"产生异常"
if__name__=="__main__":
   url="http://www.baidu.com"
   print(postHTML(url))
```



