---
title: VOC(visual object classes challenge)数据库描述 
tags: 机器学习, 深度学习
notebook: 机器学习
---
#### VOC2007

VOC2007 包含一系列生活场景中的实际图片，包含如下的20类图片：

* **Person**: person
* **Animal**: bird, cat, cow, dog, horse, sheep
* **Vehicle**: aeroplane, bicycle, boat, bus, car, motobike, train
* **Indoor**: bottle, chair, dining table, potted plant, sofa, tv/monitor

在VOC2007数据库中，提供5011张图片用于训练和验证，4952张图片用于测试。所有这些图片(5011 + 4952)，目前都提供了对应的标注文件。

VOC数据库主要用于以下两个目的：

* **Classification**: For each of the twenty classes, predicting presence/absence of  an example of that class in the test image
* **Detection**: Predicting the bounding box and label of each object from the twenty target classes in the test image


#### VOC2012

VOC2012 增加了训练图片的数量，提供了17125张图片用于训练，但仍然保持了20个类别，且类别与 VOC2007 保持一致。