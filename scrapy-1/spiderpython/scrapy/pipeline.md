# Pipeline

## `FilesPipeline`

1. Define a new  `Class FileItem(scrapy.item)` in the items.py, with 2 attributes: `file_urls` and `files`

```python
class FileItem(scrapy.Item):
    file_urls = scrapy.Field()
    files = scrapy.Field()
```

2. Change settings.py

```python
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   'scrapy.pipelines.images.FilesPipeline' : 1
}
```

3. Add the following code in the settings.py to set download path:

```python
FILE_STORE = os.path.join(os.path.dirname(os.path.dirname(__file__)),'file')
```

## `ImagesPipeline`

1. Define a new  `Class ImageItem(scrapy.item)` in the items.py, with 2 attributes: `image_urls` and `images`

```python
class ImageItem(scrapy.Item):
    image_urls = scrapy.Field()
    images = scrapy.Field()
```

2. Change settings.py

```python
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   'project_name.pipelines.Name_ImagesPipeline' : 10
}
```

3. Add the following code in the settings.py to set download path:

```python
IMAGES_STORE = os.path.join(os.path.dirname(os.path.dirname(__file__)),'images')
```

