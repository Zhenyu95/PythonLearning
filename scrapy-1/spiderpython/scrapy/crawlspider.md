# CrawlSpider

## Create new project

#### Create new Project:

```text
scrapy startproject project_name
```

#### To create CrawlSpider: 

```text
scrapy genspider -t crawl spider_name url
```

### rule

```python
rules = (
        Rule(LinkExtractor(allow=r'regex'),callback='parse_name',follow=False)
    )
```

#### callback

When the url only serves the director function \(only the urls on this page matters, other contents are not important\), do not set `callback`. 

#### follow

If the urls in this url need to be extracted too, set `follow = True` , otherwise False

## Need to notice

### allowed\_domains should not have http prefix and /

```python
allowed_domains = ['wxapp-union.com']
```



