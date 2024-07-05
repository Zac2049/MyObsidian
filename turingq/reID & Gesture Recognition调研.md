`[descfly@192.168.1.61](mailto:descfly@192.168.1.61) desc2022

`docker exec -it reid /bin/bash`
# Person re-identification
reID（行人重识别）的一般方法通常涉及以下步骤：

1. **图像采集和预处理：** 首先，从不同的监控摄像头或图像数据集中收集行人图像。这些图像可能在不同的环境下拍摄，具有不同的光照、角度和遮挡等问题。因此，在进一步处理之前，通常需要对这些图像进行预处理，如调整大小、归一化、去除噪声等。

2. **特征提取：** 从预处理后的图像中提取用于表示行人身份的特征。常见的特征提取方法包括基于手工设计的特征（如HOG、LBP）和基于深度学习的特征（如CNN）。这些特征可以捕捉到行人的外观、姿态和其他可辨识特征。

3. **特征匹配：** 将提取的特征用于比较行人图像之间的相似性。这可以通过计算特征之间的距离或相似度来完成。在特征匹配阶段，常用的方法包括欧氏距离、余弦相似度等。

4. **重识别模型训练和评估：** 使用带有已知标签的数据集对行人重识别模型进行训练。训练过程通常包括特征提取网络的训练和重识别模型的训练。为了评估模型的性能，可以使用准确率、召回率、mAP（平均精度均值）等指标。

5. **跨摄像头匹配：** 最终目标是在不同的监控摄像头之间识别出同一行人。因此，需要解决跨摄像头匹配的问题，即如何将在不同摄像头捕捉到的行人进行正确匹配。

ReID（Re-identification）是指在不同的监控摄像头之间对同一目标进行准确识别和匹配的任务。其问题定义如下：

假设我们有一个监控摄像头网络，其中包含多个摄像头。每个摄像头在不同的位置，角度和光照条件下捕获到的图像可能会有所不同。给定一个查询图像（query image），我们的目标是在所有摄像头捕获的图像中找到与查询图像中的目标最匹配的图像，从而实现目标的重新识别。

为了数学描述这个问题，我们可以定义如下符号：

- $I_q$：查询图像
- $I_g$：图库中的某个图像（gallery image）
- $f(I)$：特征提取函数，用于从图像中提取特征向量
- $d(f(I_q), f(I_g))$：特征向量之间的距离度量，如欧氏距离或余弦相似度

因此，ReID的目标可以定义为在图库中找到一个图像$I_g$，使得$d(f(I_q), f(I_g))$最小化。

``` python
# queue特征队列 f1特征提取器 f2相似度计算并匹配以及得到当前camera图像每个box的标注

class ca_object
	def __init__(self):
		self.box
		self.anno


cur_ob = ca_object()
cur_ob.box = yolov5_map(init_camera)# 初始box
init_f_queue = f1(cur_ob.box, init_camera) # per box 初始特征
init_annotations = anno_map(cur_ob.box) # per box 初始标注
cur_ob.anno = init_annotations
objects.append(cur_ob)

# 关于特征f_queue
# feature应该是fold还是per camera per feature形式

for camera in cameras :
	cur_ob = ca_object()
	cur_ob.box = yolov5_map(camera)
	cur_queue = f1(cur_ob.box, camera) # 当前特征
	cur_ob.anno = f2(f_queue, cur_queue) # 匈牙利匹配算法 or 相似度计算等方法，得到当前camera的标注, cur_queue[i].anno = f_queue[j].anno, where cur_queue[i] matches f_queue[j]
	
	f_queue = fold(f_queue, cur_queue) # 一个问题是，f2是要接受queues列表还是就一对即可，即特征是fold成一组特征还是采取特征列表的形式
	f_queue.append(cur_queue)

```

```python
import numpy as np
from scipy.optimize import linear_sum_assignment

# 假设有两个摄像头，每个摄像头检测到的目标数目不同
camera1_targets = [(1, 100), (2, 150), (3, 200)]  # (target_id, feature_vector)
camera2_targets = [(1, 120), (2, 130), (3, 180), (4, 210)]

# 计算特征向量之间的距离矩阵
dist_matrix = np.zeros((len(camera1_targets), len(camera2_targets)))
for i, (_, feature1) in enumerate(camera1_targets):
    for j, (_, feature2) in enumerate(camera2_targets):
        dist_matrix[i, j] = np.linalg.norm(feature1 - feature2)

# 使用匈牙利算法进行最佳匹配
row_ind, col_ind = linear_sum_assignment(dist_matrix)

# 输出匹配结果
for i, j in zip(row_ind, col_ind):
    target1_id, _ = camera1_targets[i]
    target2_id, _ = camera2_targets[j]
    print(f"Target {target1_id} from camera 1 matches with target {target2_id} from camera 2")

```


survey2的综述paper中提到，
>utilize the gallery-to-gallery similarity to optimize the initial ranking list

多摄像头问题，是多gallery对init query的ranking optimization问题



在ReID任务中，常见的损失函数包括Triplet Loss和Cross-Entropy Loss。这两种损失函数可以在训练过程中用于优化模型参数，以实现更好的重识别性能。

一些方案：
- paper with code这个任务的排行
	https://paperswithcode.com/task/person-re-identification
	SOLDER 阿里的项目

- https://gitcode.com/zengwb-lx/yolov5-deepsort-fastreid/tree/main
- https://github.com/JDAI-CV/fast-reid

- CLIP-Driven Fine-grained Text-Image Person Re-identification
	https://arxiv.org/abs/2210.10276
	https://arxiv.org/abs/2211.13977

- survey
	https://arxiv.org/abs/2401.06960
	https://www.semanticscholar.org/paper/Deep-Learning-for-Person-Re-Identification%3A-A-and-Ye-Shen/a6bba5ce9867c978210e3d056691b5c1e769b760


情况，按照track和detection的结果进行分类：
1. unmatched detection
2. unmatched tracks
3. matched
4. 级联匹配


deep sort本身有一个reid的系统，如果我们的query feat或者说gallery都相同，那每次计算相似度都在同一个id系统里分配




# Gesture Recognition

## Google MediaPipe
谷歌的mediapipe给了一系列方案

paper with code HGR
https://paperswithcode.com/task/hand-gesture-recognition

---
## 5.8
是否需要global_node的淘汰策略
是否需要detection的存储和读取
	什么格式，文本格式还是json之类的

` ffmpeg -framerate 24 -pattern_type glob -i '/data/kang/Yolov5-Deepsort-Fastreid/map_related/multi_cam_reid/map_view/*.jpg' -c:v mpeg4 -b:v 16M -pix_fmt yuv720p /data/kang/Yolov5-Deepsort-Fastreid/map_related/output_map_view.mp4
jpg to mp4

img和ori_img有什么区别(看看源码)

camera_detection这种数据结构怎么结合原视频draw出来
camera_detection（cade）本是视频预处理得到的，有一个全能的draw函数，得到所有的cade，per camera per frame地draw

问题：
如果需要有绘制f: cade -> drawing，实际per camera per frame地draw，per cam per frame遍历cade的所有cam_id, trk_id, frame_num, 找到global_id, bbox_xywh。

trk_id 如何决定


```python
        # {
            # cam_id:{
                # trk_id:{
                    # conf,
                    # feature:array,
                    # bbox:xywh,
                    # is_occlusion:iou,
                    # crop_img,
                    # frame_num,
                    # time_stamp},
            # }
        # }
    
        self.cameras_detection = {}

        # {
            # cam_id:{
                # frame_num:{
                    # conf,
                    # feature:array,
                    # bbox:xywh,
                    # is_occlusion:iou,
                    # crop_img,
                    # trk_id,
                    # time_stamp},
            # }
        # }
        self.cameras_detection = {}

```

改正点：
Global_Node的update_feature在更新feature的同时，也应该更新conf等其他的所有属性，因为他们是对应关系。

trk id 不变时，global id应该也不变

## 6.7
1. 检测的多判、误判和漏判->sort的结果欠佳，还是中间环节的问题
2. 后面会有很多检测结果判成同一GID，什么原因？
3. 用日志记录看看

## 6.18
![[Pasted image 20240618140742.png]]

![[Pasted image 20240618140825.png]]


每个检测出的行人特征 特征向量的可视化 特征曲线


## 7.1
rtsptorch-detectron2                                 v2.0  
创建镜像试试

conda activate detectron2
python export.py --weights yolov5s.pt --device 0  --include onnx --batch-size 16 --half \
--img 928 1088 --dynamic(不一定需要)


docker run -dit rtsptorch-detectron2:v2.0   --name lph_detectron2 -v /data/kang:/data/kang

docker run -dit --privileged --gpus all --net=host \
-v /data/kang/:/data/kang/ \
-it --name lph_detectron2 rtsptorch-detectron2:v2.0 /bin/bash

onnx
ValueError: operands could not be broadcast together with shapes (3, 918, 1088) (1, 3, 640, 640)

original reid repo img.shape torch.Size([1, 3, 640, 1088])

从detectron2到容器到reid
packaging
apex python setup.py install
faiss-gpu easydict

