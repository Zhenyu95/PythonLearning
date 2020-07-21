# OpenCV

## Basics

### Go Through all pixels

```python
for row in range(img.shape[0]):
        for col in range(img.shape[1]):
            for ch in range(img.shape[2]):
                pv = img[row,col,ch]
                img[row,col,ch] = 255 - pv
```

### Count running time

```python
t1 = cv.getTickCount()
call_function()
t1 = cv.getTickCount()
rtime = (t2-t1)/cv.getTickFrequency()
print(rtime)
```

### Use HSV to extract specific color

```python
def extract_color_green():
    capture = cv.VideoCapture("demo.mp4")
    while True:
        ret, frame = capture.read()
        if ret == False:
            break
        hsv = cv.cvtcolor(frame,cv.COLOR_BGR2HSV)
        lower_hsv = np.array([37,43,46])
        upper_hsv = np.array([77,255,255])
        mask = cv.inRange(hsv,lowerb=lower_hsv,upperb=upper_hsv)
        cv.imshow('video',frame)
        cv.imshow('mask',mask)
        c = cv.waitkey(40)
        if c ==27:
            break
```

### Split color channels

```python
img = cv.imread('demo.jpg')
#split the three channel
b, g, r = cv.split(img)
#delete specific channel: red
img[:,:, 2] = 0
#merge the channels
cv.merge(b, g, r)
```

## APIs

#### Inverse color

```python
img = cv.bitwise_not(img)
```

#### RGB 2 Gray/HSV/YUV...

```python
gray = cv.cvtcolor(img,cv.COLOR_BGR2GRAY)
hsv= cv.cvtcolor(img,cv.COLOR_BGR2HSV)
yuv = cv.cvtcolor(img,cv.COLOR_BGR2YUV)
```



