---
layout:     post
title:      OpenCV实时目标检测
category: blog
description: OpenCV can do everything
---
![opencv-cat](https://raw.githubusercontent.com/JounyWang/JounyWang.github.io/master/_posts/blog/image/opencv-cat.jpg)

如何实时识别摄像头视频中猫的出现？

我们知道，其实视频也是由一帧一帧的画面快速播放来实现运动效果的，所以可以明确一件事情：视频就是无数张图片。那么我们对每张图片分别来进行猫脸识别，是不是就可以做到实时读取摄像头画面来进行即时的猫脸识别？

******
我们来看是怎样实现的。

    import cv2
    cap = cv2.VideoCapture(0)
    cascade_path = 'D:/AI/Face/haarcascades/haarcascade_frontalcatface_extended.xml'
        while cap.isOpened():
            _, frame = cap.read()
            frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
            cascade = cv2.CascadeClassifier(cascade_path)
            facerect = cascade.detectMultiScale(frame_gray, scaleFactor=1.2, minNeighbors=3, minSize=(10, 10))
            if len(facerect) > 0:
                print('face detected')
                for rect in facerect:
                    cv2.rectangle(frame, tuple(rect[0:2]), tuple(rect[0:2] + rect[2:4]), (255, 255, 255), thickness=2)
            else:
                print('face not detected')
        cv2.imshow("capture", frame)    
        k = cv2.waitKey(100)
        if k == 27:
            break
    cap.release()
    cv2.destroyAllWindows()

******
下面就让我细细道来这些代码都发生了什么事情。

    import cv2

导入编译好的OpenCV文件

    cap = cv2.VideoCapture(0)

打开电脑摄像头

    cascade_path = 'D:/AI/Face/haarcascades/haarcascade_frontalcatface_extended.xml'

这一行是指定了我们要用的Haar分类器,这个分类器也是 OpenCV 预先训练好的，我们直接拿来用就可以啦！

    while cap.isOpened():
            _, frame = cap.read()

字面意思，当摄像头打开的时候读取视频帧，即frame

    frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

上面这行代码是将我们读取的每一个视频帧转换为灰度图片。

    cascade = cv2.CascadeClassifier(cascade_path)

这行是获取级联分类的特征量。

    facerect = cascade.detectMultiScale(frame_gray, scaleFactor=1.2, minNeighbors=3, minSize=(10, 10))

这一行就是猫咪脸部识别的执行！

    if len(facerect) > 0:

判断识别结果，如果 facerect>0 就是在视频画面里识别到猫脸啦！

    for rect in facerect:

遍历facerect。

    cv2.rectangle(frame, tuple(rect[0:2]), tuple(rect[0:2] + rect[2:4]), (255, 255, 255), thickness=2)

上面这一行是将猫脸在视频帧中的位置用白色线画出来。frame就是读取的视频帧，(255, 255, 255)即为白色，thickness 顾名思义就是白色线的宽厚度。

    cv2.imshow("capture", frame)

最后这步是实时显示猫咪脸部位置的视频帧画面。

    k = cv2.waitKey(100)
        if k == 27:
            break

监听到Esc键即退出程序

    cap.release()
    cv2.destroyAllWindows()

关闭摄像头视频流，关闭显示窗口


