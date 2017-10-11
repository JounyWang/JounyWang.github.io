---
layout:     post
title:      Tensorflow 实现多个物体同时检测
category: blog
description: The big brother is watching you.
---
今天我们来看如何使用Tensorflow实现多个目标物体同时检测。

下面是一张原始图：

![image4](https://github.com/JounyWang/JounyWang.github.io/raw/master/_posts/blog/image/image4.jpg)


下面则是我们今天要实现的效果图：

![output](https://github.com/JounyWang/JounyWang.github.io/raw/master/_posts/blog/image/output.png)

怎么样？有没有感觉很神奇？

接着就让我们来看怎样来具体实现吧。

相关依赖项：

    import numpy as np
    import os
    import six.moves.urllib as urllib
    import sys
    import tarfile
    import tensorflow as tf
    import zipfile
    from collections import defaultdict
    from io import StringIO
    from matplotlib import pyplot as plt
    from PIL import Image
    from utils import label_map_util
    from utils import visualization_utils as vis_util

模型：

    MODEL_NAME = 'ssd_mobilenet_v1_coco_11_06_2017'
    MODEL_FILE = MODEL_NAME + '.tar.gz'
    DOWNLOAD_BASE = 'http://download.tensorflow.org/models/object_detection/'

模型路径：

    PATH_TO_CKPT = MODEL_NAME + '/frozen_inference_graph.pb'

标签库路径：

    PATH_TO_LABELS = os.path.join('data', 'mscoco_label_map.pbtxt')

    NUM_CLASSES = 90

    tar_file = tarfile.open(MODEL_FILE)
    for file in tar_file.getmembers():
        file_name = os.path.basename(file.name)
        if 'frozen_inference_graph.pb' in file_name:
            tar_file.extract(file, os.getcwd())

    detection_graph = tf.Graph()
    with detection_graph.as_default():
        od_graph_def = tf.GraphDef()
        with tf.gfile.GFile(PATH_TO_CKPT, 'rb') as fid:
            serialized_graph = fid.read()
            od_graph_def.ParseFromString(serialized_graph)
            tf.import_graph_def(od_graph_def, name='')

    label_map = label_map_util.load_labelmap(PATH_TO_LABELS)
    categories = label_map_util.convert_label_map_to_categories(label_map, max_num_classes=NUM_CLASSES, use_display_name=True)
    category_index = label_map_util.create_category_index(categories)


    def load_image_into_numpy_array(image):
        (im_width, im_height) = image.size
        return np.array(image.getdata()).reshape(
            (im_height, im_width, 3)).astype(np.uint8)

待检测目标图片路径与名字：

    PATH_TO_TEST_IMAGES_DIR = 'test_images'
    TEST_IMAGE_PATHS = [ os.path.join(PATH_TO_TEST_IMAGES_DIR, 'image{}.jpg'.format(i)) for i in range(4, 5) ]

输出的图片尺寸：

    IMAGE_SIZE = (12, 8)

执行：

    with detection_graph.as_default():
        with tf.Session(graph=detection_graph) as sess:
            # 输入输出张量
            image_tensor = detection_graph.get_tensor_by_name('image_tensor:0')
            # 每个box代表图像在一个特定的对象检测的一部分。
            detection_boxes = detection_graph.get_tensor_by_name('detection_boxes:0')
            detection_scores = detection_graph.get_tensor_by_name('detection_scores:0')
            detection_classes = detection_graph.get_tensor_by_name('detection_classes:0')
            num_detections = detection_graph.get_tensor_by_name('num_detections:0')
            for image_path in TEST_IMAGE_PATHS:
                image = Image.open(image_path)
                image_np = load_image_into_numpy_array(image)
                image_np_expanded = np.expand_dims(image_np, axis=0)
                # 检测
                (boxes, scores, classes, num) = sess.run(
                    [detection_boxes, detection_scores, detection_classes, num_detections],
                    feed_dict={image_tensor: image_np_expanded})
                vis_util.visualize_boxes_and_labels_on_image_array(
                    image_np,
                    np.squeeze(boxes),
                    np.squeeze(classes).astype(np.int32),
                    np.squeeze(scores),
                    category_index,
                    use_normalized_coordinates=True,
                    line_thickness=1)
                plt.figure(figsize=IMAGE_SIZE)
                plt.imshow(image_np)

显示检测结果：

    plt.show()
