---
description: All learnings regarding dict
---

# Dict

## Dict Method

### dict.get\(key\[, default\]\):

Return the value for _**key**_ if _**key**_ ****is in the dictionary, else _**default**_. If _**default**_ ****is not given, it defaults to `None`, so that this method never raises a `KeyError`.

```python
def move(selection,direction):
        return {    
                ('T', "right"): 'P',
                ('T', "down"):  'H',
                ('P', "left"):  'T',
                ('P', "down"):  'Q',
                ('Q', 'up'):    'P',
                ('Q', 'left'):  'H',
                ('H', 'right'): 'Q',
                ('H', 'up'):    'T',
                }.get((selection, direction), selection)
```

