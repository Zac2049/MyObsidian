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

$${G(s) \triangleq \mathcal{F}\{g\}(s)=\int_{-\infty}^{\infty} g(x) e^{-i 2 \pi s x} d x, \quad s \in \mathbb{R},}$$
$$H(s) \triangleq \mathcal{F}\{h\}(s)=\int_{-\infty}^{\infty} h(x) e^{-i 2 \pi s x} d x, \quad s \in \mathbb{R}$$

$$
r(s)=\{g * h\}(s)=\mathcal{F}^{-1}\{G \cdot H\}
$$

$$X[k]=\sum_{n=0}^{N-1} x[n] e^{-j(2 \pi / N) k n}:=\sum_{n=0}^{N-1} x[n] W_N^{k n}$$

$$x[n]=\frac{1}{N} \sum_{k=0}^{N-1} X[k] e^{j(2 \pi / N) k n}$$






$$
\mathbf{z}_\mathbf{0}=\left[\mathbf{x}_{\mathrm{\mathbf{c}\mathbf{a}\mathbf{s}}};\mathbf{x}_\mathbf{p}^\mathbf{1}\mathbf{E};\mathbf{x}_\mathbf{p}^\mathbf{2}\mathbf{E};\cdots;\mathbf{x}_\mathbf{p}^\mathbf{N}\mathbf{E}\right]+\mathbf{E}_{\mathrm{\mathbf{p}\mathbf{s}}}, \mathbf{E}\in R^{\left(P^\mathbb{2}\cdot C\right)\times D},\mathbf{E}_{\mathrm{\mathbf{p}\mathbf{s}}}\in R^{\left(N+\mathbb{1}\right)\times D}
$$

$$\boldsymbol{Z}_{l}=\mathcal{F}\left[\boldsymbol{z}_{l}\right]\in\mathbb C^{H\times W}	
$$

$$\boldsymbol{{Z}_{l}}=\boldsymbol{K}\odot\boldsymbol{Z}_{l}
$$

$$\boldsymbol z_{l+1}\gets\mathcal{F}^{-1}\left[\widetilde{\boldsymbol{Z}_{l}}\right]	
$$


Problem define:

$$Method \space Define:
$$

$$y^{(i)} = \begin{cases}
1, & \text{if pixel is not 0} \\
0, & \text{otherwise}
\end{cases}
$$$$
\begin{gathered}
\hat{\boldsymbol{g}} \leftarrow \frac{1}{m} \nabla_{\boldsymbol{\theta}} \sum_i L\left(f\left(\boldsymbol{x}^{(i)} ; \boldsymbol{\theta}\right), \boldsymbol{y}^{(i)}\right) \\
\boldsymbol{\theta} \leftarrow \boldsymbol{\theta}-\epsilon_k \hat{\boldsymbol{g}}
\end{gathered}
$$
$$Inference:$$

$$
given\  g(x;\theta)),\  where \ f(x;\theta) = Softmax(g(x;\theta))\\
$$
$$for\ target\ t,\ g(t;\theta) \rightarrow [0,255] color\ space$$



## CDUM
$$P_k \in \mathbb{R}^{1 \times D \times W \times H}$$

$$\theta_k = MLP(w_k \oplus f)$$

$$P_k = \text{Sigmoid}(((F * \theta_{k1}) * \theta_{k2}) * \theta_{k3}$$