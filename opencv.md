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
        dst = cv.bitwise_and(frame,frame,mask=mask)
        cv.imshow('video',frame)
        cv.imshow('result',dst)
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

### Basic Operations

```text
#add
cv.add(img1,img2)

#subtract
cv.subtract(img1,img2)

#divide
cv.divide(img1,img2)

#multiply
cv.multiply(img1,img2)

#mean
cv.mean(img1)

#standard deviation
mean, dev = cv.meanStdDev(img)

#and
cv.bitwise_and(img1,img2)

#or
cv.bitwise_or(img1,img2)

#not
cv.bitwise_not(img)
```

### Adjust contrast and brightness

```python
def contrast_brightness(img,contrast,brightness):
    #create a blank white image to help adjust contrast and bright ness
    blank = np.zeros(img.shape,img.dtype)
    #adjusted image is the operation result of 
    #(weight of img + weight of blank)*brightness
    dst = cv.addWeighted(img, contrast, blank, 1-contrast, brightness)
    cv.imshow('contrast_brightness',dst)
```

### floodfill

```python
def floodfill(img):
    h, w = img.shape[:2]
    mask = np.zeros([h+2,w+2],np.uint8)
    #cv2.floodFill(image, mask, seedPoint, newVal[, loDiff[, upDiff[, flags]]])
    cv.floodFill(img,mask,(1,1),(255,0,0),(100,100,100),(50,50,50),cv.FLOODFILL_FIXED_RANGE)
    cv.imshow('floodfill',img)
```

## APIs

### RGB 2 Gray/HSV/YUV...

```python
gray = cv.cvtcolor(img,cv.COLOR_BGR2GRAY)
hsv= cv.cvtcolor(img,cv.COLOR_BGR2HSV)
yuv = cv.cvtcolor(img,cv.COLOR_BGR2YUV)
```



