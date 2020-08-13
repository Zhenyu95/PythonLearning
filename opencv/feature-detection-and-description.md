# Feature Detection and Description

## Harris Corner Detection

### `cv.cornerHarris()`

```python
def harrisCorner(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    
    gray = np.float32(gray)
    # cv2.cornerHarris(src, blockSize, ksize, k[, dst[, borderType]]) → dst
    dst = cv.cornerHarris(gray, 2, 3, 0.04)
    
    #result is dilated for marking the corners, not important
    dst = cv.dilate(dst,None)
    
    # Threshold for an optimal value, it may vary depending on the image.
    img[dst>0.005*dst.max()]=[0, 0, 255]
    
    cv.imshow('dst',img)
```

### `cv.cornerSubPix()`

```python
def cornerSubPix(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)

    # find Harris corners
    gray = np.float32(gray)
    dst = cv.cornerHarris(gray, 2, 3, 0.04)
    dst = cv.dilate(dst, None)
    ret, dst = cv.threshold(dst, 0.01 * dst.max(), 255, 0)
    dst = np.uint8(dst)

    # find centroids
    ret, labels, stats, centroids = cv.connectedComponentsWithStats(dst)

    # define the criteria to stop and refine the corners
    criteria = (cv.TERM_CRITERIA_EPS + cv.TERM_CRITERIA_MAX_ITER, 100, 0.001)
    # cv2.cornerSubPix(image, corners, winSize, zeroZone, criteria) → None
    corners = cv.cornerSubPix(gray, np.float32(centroids), (5, 5), (-1, -1), criteria)

    # Now draw them
    res = np.hstack((centroids, corners))
    res = np.int0(res)
    img[res[:, 1], res[:, 0]] = [0, 0, 255]
    img[res[:, 3], res[:, 2]] = [0, 255, 0]

    cv.imshow('dst',img)
```

## Shi-Tomasi corner detector \(`cv.goodFeaturesToTrack()`\)

```python
def shiTomasi(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    # cv2.goodFeaturesToTrack(image, maxCorners, qualityLevel, minDistance[, corners[, mask[, blockSize[, useHarrisDetector[, k]]]]]) → corners
    # maxCorners – Maximum number of corners to return
    # qualityLevel – Parameter characterizing the minimal accepted quality of image corners.
        # The parameter value is multiplied by the best corner quality measure, which is the minimal eigenvalue
    # minDistance – Minimum possible Euclidean distance between the returned corners.
    corners = cv.goodFeaturesToTrack(gray, 25, 0.01, 10)
    # np.int0() is alias of np.int64
    corners = np.int0(corners)

    for i in corners:
        x, y = i.ravel()
        cv.circle(img, (x, y), 3, 255, -1)

    plt.imshow(img), plt.show()
```

## SIFT

the following code is based on opencv 4.3.0

```python
def imgSIFT(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    print('gray')
    
    # has to use cv.xfeatures2d_SIFT.create(), not cv.xfeatures2d_SIFT_create()
    # not cv.SIFT.create() not cv.SIFT()
    sift = cv.xfeatures2d_SIFT.create()
    print('sift')
    kp = sift.detect(gray, None)
    print('kp,decs')
    
    # 2 ways to show the key points
    # cv.drawKeypoints(gray, kp, img)
    img = cv.drawKeypoints(gray, kp, outImage=img, flags=cv.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)
    
    cv.imshow('result.jpg', img)
```

## SURF

The following codes are ok in Anaconda Spyder with opencv version 3.4.2.17

```python
def imgSURF(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    surf = cv.xfeatures2d_SURF.create(hessianThreshold=500)

    kp, des = surf.detectAndCompute(gray, None)

    img = cv.drawKeypoints(img, kp, None, (255,0,0), 4)
    cv.imshow('result',img)
```

If the orientation is not important, disable it is much better/faster than normal method.

```python
def imgSURF(img):
    gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
    surf = cv.xfeatures2d_SURF.create(hessianThreshold=500)
    
    surf.setUpright(True)

    kp, des = surf.detectAndCompute(gray, None)

    img = cv.drawKeypoints(img, kp, None, (255,0,0), 4)
    cv.imshow('result',img)
```

## FAST

```python
def imgFAST(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    # Initiate FAST object with default values
    fast = cv.FastFeatureDetector.create()

    # find and draw the keypoints
    kp = fast.detect(gray,None)
    img = cv.drawKeypoints(gray, kp, None, color=(255,0,0))

    # Print all default params
    print( "Threshold: {}".format(fast.getThreshold()) )
    print( "nonmaxSuppression:{}".format(fast.getNonmaxSuppression()) )
    print( "neighborhood: {}".format(fast.getType()) )
    print( "Total Keypoints with nonmaxSuppression: {}".format(len(kp)) )

    cv.imshow('fast_true.png',img)

    # Disable nonmaxSuppression
    fast.setNonmaxSuppression(0)
    kp = fast.detect(img,None)

    print("Total Keypoints without nonmaxSuppression: ", len(kp))

    img3 = cv.drawKeypoints(img, kp, None, color=(255,0,0))

    cv.imshow('fast_false.png',img3)
```

