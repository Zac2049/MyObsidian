	有一些公共心脏短轴和长轴数据集可供使用，以下是其中一些数据集的引用：

### 心脏数据集

1.  Cardiac Atlas Project: 该项目提供了一个包含100个心脏磁共振图像数据集的在线资源库，其中包括短轴和长轴图像以及配准后的图像。 ([https://cardiacatlas.org/data/](https://cardiacatlas.org/data/))
2.  Sunnybrook Cardiac Data: 该数据集提供了左室短轴和四腔心长轴数据。 ([https://www.cardiacatlas.org/studies/sunnybrook-cardiac-data/](https://www.cardiacatlas.org/studies/sunnybrook-cardiac-data/))
3.  MM-WHS, ACDC, MM1&2


### LLM

1. 大模型的并行计算nvidia megatron-LM
2. 预训练（无监督）加微调
3. RLHF
4. 大模型参数不需要改变，可采用adapter（类似残差）进行下游任务

universal segmentation model, partial label dataset, language guided segmentation



### 课题
1. 用心梗cine图像输入，输出定量T1 mapping
2. 3D cine模型构建鉴别肥厚性心肌病与左室肥厚的高血压心肌病
3. 3D肌小梁重建 

## Conference Candidate
- IEEE International Symposium on Biomedical Imaging
- SPIE Medical Imaging
- ICIP：International Conference on Image Processing
- The International Conference on Medical Imaging with Deep Learning (MIDL)

### 毕设工作
肌小梁重建->分割
问题：
- 异常检测/实例分割/多模态（对比学习）分割
	- backbone可以用gfunet的内容
- 超分/重建

工程，网页可显示
