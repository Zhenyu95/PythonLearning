# Requests

## Basic:

Requests lib general code frame:

```python
import requests
def getHTMLText(url):
    try:#异常处理
        r=requests.get(url,timeout=30)
        r.raise_for_status()#如果状态不是200，引发HTTPError异常
        r.encoding=r.apparent_encoding
        return r.text
    except:
        return"产生异常"
if__name__=="__main__":
   url="http://www.baidu.com"
   print(getHTMLText(url))
```

