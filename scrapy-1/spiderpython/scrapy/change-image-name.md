# Change image name

## `items.py`

```python
import scrapy


class ImageItem(scrapy.Item):
    title = scrapy.Field()
    image_urls = scrapy.Field()
    images = scrapy.Field()
```

## settings.py

```python
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   'autohome_pic.pipelines.AutoImagesPipeline' : 1
}

#add the desired saved path
IMAGES_STORE = os.path.join(os.path.dirname(os.path.dirname(__file__)),'images')
```

## pipelines.py

```python
# -*- coding: utf-8 -*-

# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html

import os
from scrapy.pipelines.images import ImagesPipeline
from urllib.parse import urlparse
from scrapy.http import Request


class AutoImagesPipeline(ImagesPipeline):
    def get_media_requests(self,item,info):
        for image_url in item['image_urls']:
            yield Request(image_url,meta={'title':item['title']})
    def file_path(self, request, response=None, info=None):
        path = 'images/' + ''.join(request.meta['title']) + '/' + os.path.basename(urlparse(request.url).path)
        return path
```

