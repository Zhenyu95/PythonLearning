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

### RGB 2 Gray/HSV/YUV...

```python
gray = cv.cvtcolor(img,cv.COLOR_BGR2GRAY)
hsv= cv.cvtcolor(img,cv.COLOR_BGR2HSV)
yuv = cv.cvtcolor(img,cv.COLOR_BGR2YUV)
```

## Blur

### Mean filter blur

```python
def img_blur(img):
    # cv.blur(	src, ksize[, dst[, anchor[, borderType]]]	)
    dst = cv.blur(img,(3,3))
    cv.imshow('blurred',dst)
```

### Median filter blur

```python
def img_blur(img):
    #  cv2.medianBlur(src, ksize[, dst])
    dst = cv.medianBlur(img,5)
    cv.imshow('blurred',dst)
```

### Customize Kernel

```python
# cv.filter2D(	src, ddepth, kernel[, dst[, anchor[, delta[, borderType]]]]	)
# ddepth:desired depth of the destination image
# when ddepth=-1, the output image will have the same depth as the source.
# https://docs.opencv.org/3.4/d4/d86/group__imgproc__filter.html#filter_depths
def customized_kernel(img):
    kernel = np.ones([3,3],np.float32)*(-1)
    kernel[1,1] = 9
    dst = cv.filter2D(img,-1,kernel)
    cv.imshow('sharped',dst)
```

### Gaussian blur

```python
#cv2.GaussianBlur(src, ksize, sigmaX[, dst[, sigmaY[, borderType]]]) 
cv.GaussianBlur(img,(5,5),0)
```

## Edge Preserving Filter \(EPF\)

### BiLaterial Filter

```python
def biLateralEPF(img):
    #cv2.bilateralFilter(src, d, sigmaColor, sigmaSpace[, dst[, borderType]])
    dst = cv.bilateralFilter(img,0,100,15)
    cv.imshow('biEPF',dst)
```

### Meanshift Filter

```python
def meanShiftEPF(img):
    #cv2.pyrMeanShiftFiltering(src, sp, sr[, dst[, maxLevel[, termcrit]]])
    dst = cv.pyrMeanShiftFiltering(img,10,50)
    cv.imshow('meanEPF',dst)
```

## Histogram

```python
from matplotlib import pyplot

def img_hist(img):
    color = ('blue','green','red')
    for i, color in enumerate(color):
        #Calculates a histogram of a set of arrays
        hist = cv.calcHist([img],[i],None,[256],[0,256])
        pyplot.plot(hist,color=color)
        pyplot.xlim([0,256])
    pyplot.show()
```

### Use Histogram to adjust contrast

```python
#Equalizes the histogram of a grayscale image. (easy but may lead to unsatisfactory result)
def equalHist(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    cv.imshow('Gray',gray)
    dst = cv.equalizeHist(gray)
    cv.imshow('equalHist',dst)
```

```python
#Equalizes the histogram of a grayscale image. (customized)
def clache(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    #clipLimit: Threshold for contrast limiting.
    #tileGridSize:Size of grid for histogram equalization. 
        #Input image will be divided into equally sized rectangular tiles. 
        #tileGridSize defines the number of tiles in row and column.
    clache = cv.createCLAHE(clipLimit=2, tileGridSize=(8,8))
    dst = clache.apply(gray)
    cv.imshow('Clache',dst)
```

### Use histogram to compare images

```python
#RGB compare (not reliable)
def histCompare(img1,img2):
    hist1 = cv.calcHist([img1],[0,1,2],None,[16,16,16],[0,256, 0,256, 0,256])
    hist2 = cv.calcHist([img2],[0,1,2],None,[16,16,16],[0,256, 0,256, 0,256])
    match1 = cv.compareHist(hist1, hist2, cv.HISTCMP_BHATTACHARYYA)
    match2 = cv.compareHist(hist1, hist2, cv.HISTCMP_CORREL)
    match3 = cv.compareHist(hist1, hist2, cv.HISTCMP_CHISQR)
    print("Bhatta:%s, Correlation:%s, ChiSQR:%s" %(match1, match2, match3))
```

```python
#HSV method, from Opencv Offical doc
def hsv_compare(img1,img2):
    hsv_img1 = cv.cvtColor(img1,cv.COLOR_BGR2HSV)
    hsv_img2 = cv.cvtColor(img2,cv.COLOR_BGR2HSV)

    h_bins = 50
    s_bins = 60
    histSize = [h_bins,s_bins]

    h_ranges = [0,180]
    v_ranges = [0,256]
    ranges = h_ranges + v_ranges

    channels = [0,1]

    hist1 = cv.calcHist([img1],channels,None,histSize,ranges,accumulate=False)
    cv.normalize(hist1,hist1,alpha=0,beta=1,norm_type=cv.NORM_MINMAX)
    hist2 = cv.calcHist([img2],channels,None,histSize,ranges,accumulate=False)
    cv.normalize(hist2,hist2,alpha=0,beta=1,norm_type=cv.NORM_MINMAX)

    match1 = cv.compareHist(hist1, hist2, cv.HISTCMP_BHATTACHARYYA)
    match2 = cv.compareHist(hist1, hist2, cv.HISTCMP_CORREL)
    match3 = cv.compareHist(hist1, hist2, cv.HISTCMP_CHISQR)
    print("Bhatta:%s, Correlation:%s, ChiSQR:%s" %(match1, match2, match3))
```

### show 2D histogram \(hsv\)

```python
hist = cv.calcHist([hsv_img],[0,1],None,[180,256],[0,180,0,256])
plt.imshow(hist,interpolation="nearest")
plt.show()
```

## Back Project

```python
def backProjct(target,sample):
    roi_hsv = cv.cvtColor(sample,cv.COLOR_BGR2HSV)
    target_hsv = cv.cvtColor(target,cv.COLOR_BGR2HSV)

    roiHist = cv.calcHist([roi_hsv],[0,1],None,[36,48],[0,180,0,256])
    cv.normalize(roiHist,roiHist,0,255,cv.NORM_MINMAX)
    dst = cv.calcBackProject([target_hsv],[0,1],roiHist,[0,180,0,256],1)
    cv.imshow('backProject',dst)
```

### cv.normalize\(\)

```python
cv2.normalize(src[, dst[, alpha[, beta[, norm_type[, dtype[, mask]]]]]])
```

When `normType=NORM_MINMAX`, `alpha` is the Min and `beta` is the Max

## Template matching

```python
def tempMatch(src,temp):
    methods = [
        #the actual value of them are int, ranging from 0 to 5
        cv.TM_SQDIFF,
        cv.TM_SQDIFF_NORMED,
        cv.TM_CCORR,
        cv.TM_CCORR_NORMED,
        cv.TM_CCOEFF,
        cv.TM_CCOEFF_NORMED
    ]
    tempH, tempW = temp.shape[:2]
    for method in methods:
        match = cv.matchTemplate(src,temp,method)
        min_val, max_val, min_loc, max_loc = cv.minMaxLoc(match)
        #For the first two methods ( CV_SQDIFF and CV_SQDIFF_NORMED ) the best match are the lowest values. 
        #For all the others, higher values represent better matches.
        if method in [cv.TM_SQDIFF, cv.TM_SQDIFF_NORMED]:
            topleft = min_loc
        else:
            topleft = max_loc
        #draw a rectangle to highlight template matching result
        botright = (topleft[0]+tempW,topleft[1]+tempH)
        cv.rectangle(src,topleft,botright,(0,0,255),2)
        cv.imshow('method'+str(method),src)
```

## Image Thresholding \(Binary\)

### Simple thresholding: threshold set by user

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

### Otsu thresholding: threshold calculated on Otsu's binarization

```python
def otsuThresh(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_OTSU)
    # print(ret)
    titles = ['Original Img', 'Otsu']
    images = [gray, thresh]
    for i in range(2):
        plt.subplot(2,3,i+1)
        plt.imshow(images[i],'gray')
        plt.title(titles[i])
        plt.xticks([]), plt.yticks([])
    plt.show()
```

### Triangle thresholding: threshold calculated based on Triangle binarization

```python
def triangleThresh(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_TRIANGLE)
    # print(ret)
    titles = ['Original Img', 'Otsu']
    images = [gray, thresh]
    for i in range(2):
        plt.subplot(2,3,i+1)
        plt.imshow(images[i],'gray')
        plt.title(titles[i])
        plt.xticks([]), plt.yticks([])
    plt.show()
```

### Adaptive thresholding

```python
def adptiveThres(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, th1 = cv.threshold(gray, 127, 255, cv.THRESH_BINARY)
    # cv.adaptiveThreshold(	src, maxValue, adaptiveMethod, thresholdType, blockSize, C[, dst])
    # blockSize must be odd number
    th2 = cv.adaptiveThreshold(gray, 255, cv.ADAPTIVE_THRESH_MEAN_C, cv.THRESH_BINARY, 11, 2)
    th3 = cv.adaptiveThreshold(gray, 255, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 11, 2)
    titles = ['Original Image', 'Global Thresholding (v = 127)',
              'Adaptive Mean Thresholding', 'Adaptive Gaussian Thresholding']
    images = [gray, th1, th2, th3]
    for i in range(4):
        plt.subplot(2, 2, i + 1), plt.imshow(images[i], 'gray')
        plt.title(titles[i])
        plt.xticks([]), plt.yticks([])
    plt.show()
```

## Gaussian Pyramid

```python
def gaussainPyramid(img):
    temp = img.copy()
    gaussianPyrImg = [img]
    for i in range(6):
        temp = cv.pyrDown(temp)
        # cv.imshow('Gaussian'+str(i), temp)
        gaussianPyrImg.append(temp)
    return gaussianPyrImg
```

### Laplacian Pyramid

```python
def laplacianPyramid(img):
    gaussianPyrImg = gaussainPyramid(img)
    laplacianPyrImg = [gaussianPyrImg[5]]
    for i in range(5, 0, -1):
        GE = cv.pyrUp(gaussianPyrImg[i])
        L = cv.subtract(gaussianPyrImg[i - 1], GE)
        #cv.imshow('Laplacian'+str(i),L)
        laplacianPyrImg.append(L)
    return laplacianPyrImg
```

## Image gradient

### Sobel

```python
def sobel(img):
    grad_x = cv.Sobel(img, cv.CV_64F, 1, 0, ksize=5)
    grad_y = cv.Sobel(img, cv.CV_64F, 0, 1, ksize=5)
    grad_xy = cv.addWeighted(grad_x, 0.5, grad_y, 0.5, 0)

    plt.subplot(2, 2, 1), plt.imshow(cv.cvtColor(img, cv.COLOR_BGR2RGB))
    plt.title('Original'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 2), plt.imshow(grad_x)
    plt.title('Sobel X'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 3), plt.imshow(grad_y)
    plt.title('Sobel Y'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 4), plt.imshow(grad_xy)
    plt.title('Sobel XY'), plt.xticks([]), plt.yticks([])

    plt.show()
```

### Scharr

more sensitive than Sobel

```python
def scharr(img):
    grad_x = cv.Scharr(img, cv.CV_64F, 1, 0)
    grad_y = cv.Scharr(img, cv.CV_64F, 0, 1)
    grad_xy = cv.addWeighted(grad_x, 0.5, grad_y, 0.5, 0)

    plt.subplot(2, 2, 1), plt.imshow(cv.cvtColor(img, cv.COLOR_BGR2RGB))
    plt.title('Original'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 2), plt.imshow(grad_x)
    plt.title('Scharr X'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 3), plt.imshow(grad_y)
    plt.title('Scharr Y'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 4), plt.imshow(grad_xy)
    plt.title('Scharr XY'), plt.xticks([]), plt.yticks([])

    plt.show()
```

### Laplacian

```python
def laplacian(img):
    lapImg = cv.Laplacian(img,cv.CV_64F,ksize=1)
    plt.subplot(1, 2, 1), plt.imshow(cv.cvtColor(img, cv.COLOR_BGR2RGB))
    plt.title('Original'), plt.xticks([]), plt.yticks([])
    plt.subplot(1, 2, 2), plt.imshow(lapImg)
    plt.title(' Laplacian'), plt.xticks([]), plt.yticks([])

    plt.show()
```

### To show the image after Sobel/Scharr/Laplacian:

```python
imgToShow = cv.convertScaleAbs(img)
cv.imshow('show',imgToShow)
```

## Canny Edge Detection

```python
def canny(img):
#cv2.Canny(image, threshold1, threshold2[, edges
#[, apertureSize[, L2gradient]]]) → edges
#img should be gray
#threshold2 should be 2 or 3*threshold1
    edges = cv.Canny(img,50,150)

    plt.subplot(121),plt.imshow(cv.cvtColor(img,cv.COLOR_BGR2RGB))
    plt.title('Original Image'), plt.xticks([]), plt.yticks([])
    plt.subplot(122),plt.imshow(edges,cmap = 'gray')
    plt.title('Edge Image'), plt.xticks([]), plt.yticks([])

    plt.show()
```

## Hough Line detection

```python
def hough(img):
    # must convert to gray scale first
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    # use Canny to get the edges
    edges = cv.Canny(gray, 60, 120, apertureSize=3)
    # cv.imwrite('binary.jpg', edges)
    # get the lines, the forth parameter sets the min "votes" for a line
    lines = cv.HoughLines(edges, 1, np.pi / 180, 350, None, 0, 0)
    # draw the lines on the image and write the image
    for line in lines:
        rho, theta = line[0]
        a = np.cos(theta)
        b = np.sin(theta)
        x0 = a * rho
        y0 = b * rho
        x1 = int(x0 + 1000 * (-b))
        y1 = int(y0 + 1000 * (a))
        x2 = int(x0 - 1000 * (-b))
        y2 = int(y0 - 1000 * (a))
        cv.line(img, (x1, y1), (x2, y2), (0, 0, 255), 2)
    cv.imwrite('result.jpg', img)
```

## Hough Circle detection

```python
def houghCircle(img):
    img1 = cv.medianBlur(cv.cvtColor(img,cv.COLOR_BGR2GRAY),5)
    img2 = cv.cvtColor(cv.pyrMeanShiftFiltering(img, 10, 100),cv.COLOR_BGR2GRAY)
    img3 = cv.GaussianBlur(cv.cvtColor(img,cv.COLOR_BGR2GRAY),(7,7),2,2)

    cimg1 = cv.cvtColor(img1,cv.COLOR_GRAY2BGR)
    cimg2 = cv.cvtColor(img2, cv.COLOR_GRAY2BGR)
    cimg3 = cv.cvtColor(img3, cv.COLOR_GRAY2BGR)

    circles1 = cv.HoughCircles(img1, cv.HOUGH_GRADIENT, 1, 50, param1=200, param2=90, minRadius=20, maxRadius=0)
    circles2 = cv.HoughCircles(img2, cv.HOUGH_GRADIENT, 1, 50, param1=200, param2=85, minRadius=20, maxRadius=0)
    circles3 = cv.HoughCircles(img3, cv.HOUGH_GRADIENT, 1, 50, param1=200, param2=90, minRadius=20, maxRadius=0)

    circles1 = np.uint16(np.around(circles1))
    circles2 = np.uint16(np.around(circles2))
    circles3 = np.uint16(np.around(circles3))

    for i in circles1[0,:]:
        # draw the outer circle
        cv.circle(cimg1,(i[0],i[1]),i[2],(0,0,255),5)
        # draw the center of the circle
        cv.circle(cimg1,(i[0],i[1]),2,(0,0,255),5)
    for i in circles2[0,:]:
        # draw the outer circle
        cv.circle(cimg2,(i[0],i[1]),i[2],(0,0,255),5)
        # draw the center of the circle
        cv.circle(cimg2,(i[0],i[1]),2,(0,0,255),5)
    for i in circles3[0,:]:
        # draw the outer circle
        cv.circle(cimg3,(i[0],i[1]),i[2],(0,0,255),5)
        # draw the center of the circle
        cv.circle(cimg3,(i[0],i[1]),2,(0,0,255),5)

    plt.subplot(2, 2, 1), plt.imshow(cv.cvtColor(img, cv.COLOR_BGR2RGB))
    plt.title('Original'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 2), plt.imshow(cimg1)
    plt.title('Median'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 3), plt.imshow(cimg2)
    plt.title('Mean'), plt.xticks([]), plt.yticks([])
    plt.subplot(2, 2, 4), plt.imshow(cimg3)
    plt.title('Gaussian'), plt.xticks([]), plt.yticks([])
    plt.show()
```

## Contours

### findContours\(\) & drawContours\(\)

```python
def contours(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    ret1, thresh1 = cv.threshold(gray, 127, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)
    ret2, thresh2 = cv.threshold(gray, 127, 255, 0)
    thresh3 = cv.adaptiveThreshold(gray, 255, cv.ADAPTIVE_THRESH_MEAN_C, cv.THRESH_BINARY, 11, 2)
    thresh4 = cv.adaptiveThreshold(gray, 255, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 11, 2)

    contours1, hierarchy1 = cv.findContours(thresh1, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE)
    contours2, hierarchy2 = cv.findContours(thresh2, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE)
    contours3, hierarchy3 = cv.findContours(thresh3, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE)
    contours4, hierarchy4 = cv.findContours(thresh4, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE)
    contours5, hierarchy5 = cv.findContours(thresh4, cv.RETR_TREE, cv.CHAIN_APPROX_NONE)
    
    img = np.zeros(img.shape,dtype=np.uint8)
    
    img1 = cv.drawContours(img, contours1, -1, (0, 255, 0), 3)
    img2 = cv.drawContours(img, contours2, -1, (0, 255, 0), 3)
    img3 = cv.drawContours(img, contours3, -1, (0, 255, 0), 3)
    img4 = cv.drawContours(img, contours4, -1, (0, 255, 0), 3)
    img5 = cv.drawContours(img, contours5, -1, (0, 255, 0), 3)

    titles = ['Original', 'Ostu+Simple', 'Simple+Simple', 'AdaptiveMean+Simple', 'AdaptiveGaussian+Simple',
              'AdaptiveGaussian+None']
    imgs = [img,img1,img2,img3,img4,img5]

    for i in range(6):
        plt.subplot(2,3,i+1)
        plt.imshow(imgs[i])
        plt.title(titles[i])
        plt.xticks([]), plt.yticks([])
    plt.show()
```

### Get Image Moments

```python
img = cv.imread('image.jpg',0)
ret,thresh = cv.threshold(img,127,255,0)
contours,hierarchy = cv.findContours(thresh, 1, 2)

cnt = contours[0]
M = cv.moments(cnt)
```

### Get Contour Area

```python
img = cv.imread('image.jpg',0)
ret,thresh = cv.threshold(img,127,255,0)
contours,hierarchy = cv.findContours(thresh, 1, 2)

cnt = contours[0]
area = cv.contourArea(cnt)
```

### Get Contour Perimeter

```python
img = cv.imread('image.jpg',0)
ret,thresh = cv.threshold(img,127,255,0)
contours,hierarchy = cv.findContours(thresh, 1, 2)

cnt = contours[0]
# pass True if the contours is closed.
perimeter = cv.arcLength(cnt,True)
```

### Contour Approximation

```python
img = cv.imread('image.jpg',0)
ret,thresh = cv.threshold(img,127,255,0)
contours,hierarchy = cv.findContours(thresh, 1, 2)

cnt = contours[0]
epsilon = 0.01*cv.arcLength(cnt,True)
# epsilon is the maximum distance from contour to approximated contour
approx = cv.approxPolyDP(cnt,epsilon,True)
```

### Bounding rectangle

#### Straight rectangle

```python
img = cv.imread('image.jpg',0)
ret,thresh = cv.threshold(img,127,255,0)
contours,hierarchy = cv.findContours(thresh, 1, 2)

cnt = contours[0]

x,y,w,h = cv2.boundingRect(cnt)
img = cv2.rectangle(img,(x,y),(x+w,y+h),(0,255,0),2)
```

#### Rotated rectangle

```python
img = cv.imread('image.jpg',0)
ret,thresh = cv.threshold(img,127,255,0)
contours,hierarchy = cv.findContours(thresh, 1, 2)

cnt = contours[0]

rect = cv2.minAreaRect(cnt)
box = cv2.boxPoints(rect)
box = np.int0(box)
im = cv2.drawContours(im,[box],0,(0,0,255),2)
```

## Morphological Transformations \(形态学\)

Erode, Dilation, etc not only supports binary image, but also gray image and 3 channels image

### Erode

```python
def erosion(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_OTSU)
    
    kernel = np.ones((3, 3),np.uint8)
    erosion = cv.erode(thresh, kernel)
```

### Dilation

```python
def dilation(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_OTSU)
    
    kernel = np.ones((3, 3),np.uint8)
    dilation= cv.dilate(thresh, kernel)
```

### Opening

Opening \(`cv.morphologyEX(img,cv.MORPG_OPEN,kernel)`\) is erosion followed by dilation. It is useful in removing noise

```text
def opening(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_OTSU)
    
    kernel = np.ones((3, 3), np.uint8)
    opening = cv.morphologyEx(thresh, cv.MORPH_OPEN, kernel)
```

### Closing

Closing \(`cv.morphologyEX(img,cv.MORPG_CLOSE,kernel)`\) is dilation followed by erosion. It is useful in closing small holes inside the foreground objects, or small black points on the object.

```python
def closing(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_OTSU)
    
    kernel = np.ones((3, 3), np.uint8)
    closing= cv.morphologyEx(thresh, cv.MORPH_CLOSE, kernel)
```

### Morphological gradient

`cv.morphologyEX(img,cv.MORPG_GRADIENT,kernel)` is the difference between dilation and erosion. It looks like the outline of the object

```python
def closing(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY+cv.THRESH_OTSU)
    
    kernel = np.ones((3, 3), np.uint8)
    gradient= cv.morphologyEX(thresh,cv.MORPH_GRADIENT,kernel))
```

### Customize Kernel

```python
# Rectangular Kernel
kernel = cv.getStructuringElement(cv2.MORPH_RECT,(5,5))
# Elliptical Kernel
kernel = cv.getStructuringElement(cv2.MORPH_ELLIPSE,(5,5))
# Cross-shaped Kernel
kernel = cv.getStructuringElement(cv2.MORPH_CROSS,(5,5))
```

## WaterShed

The overall procedure is:

read image -&gt; \(noise removal\) -&gt; convert to gray -&gt; threshholding -&gt; \(noise removal\) -&gt; dilate to get sure bg -&gt; distance transform to get sure fg -&gt; find unknown area -&gt; marker -&gt; watershed

```python
def waterShed(img):
    # EPF Noise removal
    img = cv.pyrMeanShiftFiltering(img, 5, 20)
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    ret, thresh = cv.threshold(gray,127,255,cv.THRESH_BINARY_INV+cv.THRESH_OTSU)
    cv.imshow('binary',thresh)

    # noise removal
    kernel = cv.getStructuringElement(cv.MORPH_RECT, (3, 3))
    closing = cv.morphologyEx(thresh, cv.MORPH_CLOSE, kernel, iterations=4)
    cv.imshow('closing', closing)

    # sure background area
    sure_bg = cv.dilate(thresh, kernel, iterations=3)
    cv.imshow('sure bg',sure_bg)
    # find sure background area
    distance_transform = cv.distanceTransform(thresh, cv.DIST_L2, 5)
    cv.imshow('distance transform',distance_transform)
    ret, sure_fg = cv.threshold(distance_transform, 0.9*distance_transform.max(), 255, 0)
    cv.imshow('sure fg',sure_fg)

    # find unknown region
    sure_fg = np.uint8(sure_fg)
    unknown = cv.subtract(sure_bg, sure_fg)
    cv.imshow('unknown',unknown)

    # Marker labelling
    ret, markers = cv.connectedComponents(sure_fg)

    # Add one to all labels so that sure background is not 0, but 1
    markers = markers + 1

    # Now, mark the region of unknown with zero
    markers[unknown == 255] = 0

    markers = cv.watershed(img, markers)
    img[markers == -1] = [255, 0, 0]
```

## grabCut

grabCut rectangle

```python
def grabCutRec(img):
    mask = np.zeros(img.shape[:2], np.uint8)

    bgdModel = np.zeros((1, 65), np.float64)
    fgdModel = np.zeros((1, 65), np.float64)

    rect = (220, 1, 1000, 824)
    cv.grabCut(img, mask, rect, bgdModel, fgdModel, 5, cv.GC_INIT_WITH_RECT)

    mask = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
    img = img * mask[:, :, np.newaxis]

    plt.imshow(cv.cvtColor(img,cv.COLOR_BGR2RGB)), plt.colorbar(), plt.show()
```













