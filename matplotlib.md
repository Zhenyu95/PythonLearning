# matplotlib

## pyplot

### show histogram

```python
pyplot.plot(hist,color=color)
pyplot.xlim([0,256])
pyplot.show()
```

### 2D histogram

```python
plt.imshow(hist,interpolation="nearest")
plt.show()
```

### subplots

```python
def simThresh(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    # cv.THRESH_BINARY: thresholded img = 255 if dst(x,y)>127 else 0
    ret, thresh1 = cv.threshold(gray, 127, 255, cv.THRESH_BINARY)
    # cv.THRESH_BINARY_INV: thresholded img = 255 if dst(x,y)>127 else 0
    ret, thresh2 = cv.threshold(gray, 127, 255, cv.THRESH_BINARY_INV)
    # cv.THRESH_TRUNC: thresholded img = threshold (127 in this case) if dst(x,y)>127 else src(x,y)
    ret, thresh3 = cv.threshold(gray, 127, 255, cv.THRESH_TRUNC)
    # cv.THRESH_TOZERO: thresholded img = src(x,y) if dst(x,y)>127 else 0
    ret, thresh4 = cv.threshold(gray, 127, 255, cv.THRESH_TOZERO)
    # cv.THRESH_TOZERO: thresholded img = 0 if dst(x,y)>127 else src(x,y)
    ret, thresh5 = cv.threshold(gray, 127, 255, cv.THRESH_TOZERO_INV)
    titles = ['Original Image', 'BINARY', 'BINARY_INV', 'TRUNC', 'TOZERO', 'TOZERO_INV']
    images = [gray, thresh1, thresh2, thresh3, thresh4, thresh5]
    for i in range(6):
        plt.subplot(2, 3, i + 1)
        plt.imshow(images[i], 'gray')
        plt.title(titles[i])
        plt.xticks([]), plt.yticks([])
    plt.show()
```

### Scatter plot example

```python
def plot_diff(y_true, y_pred, title=''):
    # plot a scatter plot, y_true as x, y_pred as y
    plt.scatter(y_true, y_pred)
    # set the title
    plt.title(title)
    # set the x-axis label
    plt.xlabel('True Values')
    # set the y-axis label
    plt.ylabel('Predictions')
    # set the axis property
    plt.axis('equal')
    # set the axis property
    plt.axis('square')
    # set the x-axis limit
    plt.xlim(plt.xlim())
    # set the y-axis limit
    plt.ylim(plt.ylim())
    # plot a straight line as reference
    plt.plot([-100, 100], [-100, 100])
    plt.show()
```

