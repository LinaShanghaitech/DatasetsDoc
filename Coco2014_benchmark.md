## Coco 2014 Benchmark

### Dataset overview
- COCO数据集有91类，虽然比ImageNet和SUN类别少，但是每一类的图像多，这有利于获得更多的每类中位于某种特定场景的能力，对比PASCAL VOC，其有更多类和图像。
- COCO数据集分两部分发布，前部分于2014年发布，后部分于2015年，
- 2014年版本：82,783 training, 40,504 validation, and 40,775 testing images，有270k的segmented people和886k的segmented object；
- 2015年版本：165,482 train, 81,208 val, and 81,434 test images。

#### Data Format
1. [link address](http://cocodataset.org/#format-data)
2. Details:COCO has five annotation types: for object detection, keypoint detection, stuff segmentation, panoptic segmentation, and image captioning.All annotations share the same basic data structure below:data_format
![](https://github.com/LinaShanghaitech/DatasetsDoc/blob/master/figure/coco1.png)
#### For keypoints
- data.keys()  
  [u’info’, u’images’, u’licenses’, u’annotations’, u’categories’]
- data[‘categories’]  
  [{u’supercategory’: u’person’, u’skeleton’: [[16, 14], [14, 12], [17, 15], [15, 13], [12, 13], [6, 12], [7, 13], [6, 7], [6, 8], [7, 9], [8, 10], [9, 11], [2, 3], [1, 2], [1, 3], [2, 4], [3, 5], [4, 6], [5, 7]], u’id’: 1,
  u’keypoints’: [u’nose’, u’left_eye’, u’right_eye’, u’left_ear’, u’right_ear’, u’left_shoulder’, u’right_shoulder’, u’left_elbow’, u’right_elbow’, u’left_wrist’, u’right_wrist’, u’left_hip’, u’right_hip’,
  u’left_knee’, u’right_knee’, u’left_ankle’, u’right_ankle’], u’name’: u’person’}]
- data[‘images’][0].keys()  
  [u’license’, u’file_name’, u’coco_url’, u’height’, u’width’, u’date_captured’, u’flickr_url’, u’id’]
- data[‘annotations’][0].keys()  
  [u’segmentation’, u’num_keypoints’, u’area’, u’iscrowd’, u’keypoints’, u’image_id’, u’bbox’, u’category_id’, u’id’]
  ![](https://github.com/LinaShanghaitech/DatasetsDoc/blob/master/figure/coco2.png)

**Keypoints Detection** 
> contains all the data of the object annotation (including id, bbox, etc.)and two additional fields. First, “keypoints” is a length 3k array where k is the total number of keypoints defined for the category.
Each keypoint has a 0-indexed location x,y and a visibility flag v defined as v=0: not labeled (in which case x=y=0),v=1: labeled but not visible, and v=2: labeled and visible. 
A keypoint is considered visible if it falls inside the object segment. “num_keypoints” indicates the numberof labeled keypoints (v>0) for a given object (many objects, e.g. crowds and small objects, will have num_keypoints=0). Finally, for each category, the categories struct hastwo additional fields: “keypoints,” which is a length k array of keypoint names, and “skeleton”, which defines connectivity via a list of keypoint edge pairs and is used for visualization.
Currently keypoints are only labeled for the person category (for most medium/large non-crowd person instances)
![](https://github.com/LinaShanghaitech/DatasetsDoc/blob/master/figure/coco3.png)

**Object Detection** 
> contains a series of fields, including the category id and segmentation mask of the object. The segmentation format depends on whether the instance represents a single object (iscrowd=0 in which case polygons are used) or a collection of objects (iscrowd=1 in which case RLE is used). Note that a single object (iscrowd=0) may require multiple polygons, for example if occluded. Crowd annotations (iscrowd=1) are used to label large groups of objects (e.g. a crowd of people). In addition, an enclosing bounding box is provided for each object (box coordinates are measured from the top left image corner and are 0-indexed). Finally, the categories field of the annotation structure stores the mapping of category id to category and supercategory names.
