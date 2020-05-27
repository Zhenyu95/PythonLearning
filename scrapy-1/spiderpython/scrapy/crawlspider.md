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

## Need to notice

### allowed\_domains should not have http prefix and /

```python
allowed_domains = ['wxapp-union.com']
```



