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

# if mask is True then the element is from fg, otherwise from bg
result = [
    bg_pix if mask_pix else fg_pix
    for (bg_pix, mask_pix, fg_pix) in zip(
    (p for row in bg for p in row),
    (p for row in mask for p in row),
    (p for row in fg for p in row)
    )]
```

