# Scrapy

## Basics:

Offical doc: [https://docs.scrapy.org/en/latest/index.html](https://docs.scrapy.org/en/latest/index.html)

![](../../../.gitbook/assets/scrapy_architecture.png)

### Create New Project

#### Create project:  

Run following code in the cmd/Terminal

```python
scrapy startproject project_name
```

#### Create spider:

Run following code in the cmd/Terminal

```python
scrapy genspider spider_name url
```

#### Settings change in settings.py

Change settings.py the robot rules to False:

```python
# Obey robots.txt rules
ROBOTSTXT_OBEY = False
```

Change settings.py headers:

```python
# Override the default request headers:
DEFAULT_REQUEST_HEADERS = {
  'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
  'Accept-Language': 'en',
  'user-agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36'
}
```

#### Run Scrapy

Run following code in the cmd/Terminal

```python
scrapy crawl spider_name
```

#### Creating .py to simplify debug \(avoid using cmd/Terminal\)

Create a new .py with the following content:

```python
from scrapy import cmdline
cmdline.execute('scrapy crawl spider_name'.split())
```

### XPATH & CSS selector

The result type of XPATH/CSS selector is SelectorList/Selector which have the following method:

```python
def getall(self):
    """
    Call the ``.get()`` method for each element is this list and return
    their results flattened, as a list of unicode strings.
    """
    return [x.get() for x in self]
extract = getall

def get(self, default=None):
    """
    Return the result of ``.get()`` for the first element in this list.
    If the list is empty, return the default value.
    """
    for x in self:
        return x.get()
    return default
extract_first = get

```

### pipelines.py

To enable pipelines.py, change the setting in the settings.py:

```python
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   'project_name.pipelines.project_namePipeline': 300,
}
```

_`open_spider(self, spider)`_ is called when the **spider is opened.**

_`process_item(self, item, spider)`_ is called for **every item pipeline component.**

_`close_spider(self, spider)`_ is called when the **spider is closed**.

### items.py

use items.py define item, it's a dictionary-like class

### JsonItemExporter & JsonLinesItemExporter

When exporting result to JSON, can use these 2 methods.

_`JsonItemExporter`_: use larger memory to store all results and write when finished

_`JsonLinesItemExporter`_: write each line after process, use less memory

### Use meta to pass info

At the end of `parse(self, response)`

```python
def parse(self, response):
    ......
    yield scrapy.Request(url, 
        callback=self.parse_tocall, 
        meta={'name':value})
        
def parse_tocall(self, response):
    ......
    value=response.meta['name']
    return ...
```

