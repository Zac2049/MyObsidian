## Universal Model research
### CDUM
心脏子结构一般有：LV, RV, LA, RA, Myo, AO, PA
- spatial_dim应该是固定的项，2d心脏
	- 对于2d心脏数据集，有：
	- ACDC, 100+50, cine-MRI 1:LV 2:Myo 3:RV![[Pasted image 20230411202233.png]]
	- test has not patient 109 117 134 146
	- MM1[M&Ms challenge (ub.edu)](https://www.ub.edu/mnms/)
		- "numTest": 200,    "numTraining": 175, MR
	- MM2[M&Ms-2 Challenge (ub.edu)](https://www.ub.edu/mnms-2/)
		- "numTest": 200,    "numTraining": 160, MR
		- 1:LV 2:Myo 3:RV
- 3d心脏
	- "MMWHSCT2017",[Multi-Modality Whole Heart Segmentation Challenge (fudan.edu.cn)](http://www.sdspeople.fudan.edu.cn/zhuangxiahai/0/mmwhs/)
		"numTest": 40,    "numTraining": 20,
	- "MMWHSMRI2017",
	    "numTest": 40,    "numTraining": 20,
	-  "CHD",
	    "numTest": 14,     "numTraining": 54,
		"labels": {
	        "0": "background",
	        "1": "LV",
	        "2": "RV",
	        "3": "LA",
	        "4": "RA",
	        "5": "MYO",
	        "6": "AO",
	        "7": "PA"
	    },
ORGAN_NAME = ['Left Ventricle Blood Cavity', 'Myocardium', 'Right Ventricle Blood Cavity', 'Left Atrium Blood Cavity', 'Right Atrium Blood Cavity', 'Ascending Aorta', 'Pulmonary Artery']
	- MSD
		20 Training + 10 Testing
		1 : LA

*source code*

## MedSAM and Med SAM Adaptor

- 拿MMWHS（LV, RV, LA, RA, Myo, AO, PA）微调或者adapt已训练ACDC, MNMS的模型，在只分割LV, RV, MYO上比单纯用ACDC+MNMS的要好一些，尽管整个过程是2d训练，但在长短轴分割上也会十分有效，而且也不会有3d训练耗时和显存的问题。
- SAM开源代码并未提供language的prompt，在一些医学SAM增加文本信息。
- prompt没有足够的监督信息，用自监督训练引导监督训练。
