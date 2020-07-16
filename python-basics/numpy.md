---
description: Learnings about Numpy
---

# Numpy

## Compare 2 ndarray and merge into 1

```python
import numpy as np
fg = np.random.randint(0,2,(8,8,3))
bg = np.ones((8,8,3),dtype = np.uint8)
mask = np.all(fg == 0, axis = 2)
```

