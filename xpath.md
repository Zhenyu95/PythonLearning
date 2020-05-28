# XPATH

## Notes

**Remember to use '.' if want to continue search under the parent node**

```python
parent = response.xpath('//div')
for each in parent:
    title = each.xpath('.//a')
```



