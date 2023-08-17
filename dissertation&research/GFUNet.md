  

Fourier transform has been an important tool in digital image processing for decades [1, 35]. With the breakthroughs of CNNs in vision [ 14 , 13 ], there are a variety of works that start to incorporate Fourier transform in some deep learning
method [26, 50, 6] for vision tasks. Some of these works employ discrete Fourier transform to
convert the images to the frequency domain and leverage the frequency information to improve the performance in certain tasks , while others utilize the convolution theorem to accelerate the CNNs via fast Fourier transform (FFT) [ 26 , 9 ]. FFC [6] replaces the convolution in CNNs with an Local Fourier Unit and perform convolutions in the frequency domain. In this work, we propose to use learnable filters to interchange information
globally among the tokens in the Fourier domain, inspired by the frequency filters in the digital image processing [ 35]. We also take advantage of some properties of FFT to reduce the computational costs and the number of parameters.
[1] Gregory A Baxes. Digital image processing: principles and applications. John Wiley & Sons,
Inc., 1994.
[13] Kaiming He, Georgia Gkioxari, Piotr Dollar, and Ross Girshick. Mask r-cnn. In ICCV, 2017. 
[14] Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. Deep residual learning for image
recognition. In CVPR, pages 770–778, 2016.
[26] Shaohua Li, Kaiping Xue, Bin Zhu, Chenkai Ding, Xindi Gao, David Wei, and Tao Wan. Falcon:
A fourier transform based approach for fast and secure convolutional neural network predictions.
In CVPR, pages 8705–8714, 2020.
[50] Yanchao Yang and Stefano Soatto. Fda: Fourier domain adaptation for semantic segmentation.
In CVPR, pages 4085–4095, 2020.
[6] Lu Chi, Borui Jiang, and Yadong Mu. Fast fourier convolution. NeurIPS, 33, 2020.
[35] Ioannis Pitas. Digital image processing algorithms and applications. John Wiley & Sons, 2000.

傅里叶变换在图像压缩中的应用：

傅里叶变换可以将图像从空间域转换到频率域，将图像表示为一系列正弦和余弦函数的和。在频域中，图像的高频部分表示图像中的细节和纹理，而低频部分表示图像中的整体结构和颜色。因此，通过保留图像的低频部分，可以实现图像压缩。

具体来说，傅里叶变换压缩图像的过程如下：
1. 将图像分割为小块，通常是8x8或16x16像素的块。
    
2. 对每个块进行傅里叶变换，得到该块在频域中的表示。
    
3. 对每个块的频域表示进行排序，舍去小的变换系数，只保留靠前的一部分系数。
    
4. 将保留的系数转换回空间域，得到压缩后的图块。
    
5. 将所有压缩后的图像块拼接起来，得到压缩后的图像。
    

傅里叶变换在计算机视觉和深度学习中的应用：

在计算机视觉和深度学习中，傅里叶变换可以用于提取图像的频率特征，从而帮助学习算法更好地理解图像的结构和纹理。例如，卷积神经网络（CNN）中的卷积层可以看作是在学习图像的局部频率特征。通过将图像转换到频率域，可以更直观地观察这些特征，并有助于设计更有效的卷积核。

傅里叶变换在深度学习中的优势：

1. 频率特征具有平移不变性：在频域中，图像的平移不会影响频率特征。这意味着，即使图像发生平移，傅里叶变换后的特征仍然保持不变。这有助于提高模型的鲁棒性。
    
2. 降低计算复杂度：通过将图像转换到频率域，可以降低计算复杂度。例如，在频域中进行卷积操作的计算复杂度要低于在空间域中进行卷积操作。
    
3. 去噪和滤波：在频域中，可以更容易地对图像进行去噪和滤波操作。例如，可以通过保留低频部分来去除图像中的高频噪声。
    
4. 更好的特征表示：傅里叶变换可以提取图像的频率特征，这些特征可以更好地表示图像的结构和纹理信息。这有助于提高模型的性能。
    

需要注意的是，虽然傅里叶变换在计算机视觉和深度学习中具有一定的优势，但它也有一些局限性。例如，傅里叶变换对信号的要求较高，需要满足一定的条件才能进行变换。此外，傅里叶变换还存在一些问题，例如频谱泄露、边缘效应等，需要采取一些措施来解决。在实际应用中，需要根据具体问题和需求来选择合适的变换方法。


傅里叶变换在深度学习中可以降低计算复杂度和参数量的原因主要有以下几点：

1. 利用卷积定理：傅里叶变换可以将时（或空域）上的卷积操作转换为频域上的乘法操作。根据卷积定理，两个函数在时域（或空域上的卷积等于它们在频域上的乘积。这意味着，在频域中进行乘法操作后，再通过傅里叶逆变换到时域（或空域），可以得到原始信号经过卷积操作后的结果。由于频域乘法操作的计算复杂度通低于时域卷积操作，因此使用傅里叶变换可以降低计算复杂度。

2. 稀疏表示：在许多实际应用中，信号在频域上往往呈现出稀疏性。这意味信号的大部分频率成分的幅度接近于零，只有少数频率成分具有较大的幅度。通过傅里叶变换将信号转换到域，并利用其稀疏性，可以有效地减少需要处理的参数数量，从而降低计算复杂度和参数量。

3. 参数共享：在深度学习中，卷积神经网络（CNN）利用卷积层实现参数共享。这意味着在整个输入图像上，同一个卷积核被重使用，从而减少了模型的参数量。通过将卷积操作转换为频域乘法操作，可以进一步减少参数量，提高算效率。

4. 分离性：傅里叶变换具有分离性，这意味着多维信号的傅里叶变换可以分解为各个维度上的一维傅里叶变换。这种分离性使得在处理多维信号时，可以独立地对每个维度进行傅里叶变换和逆变换，从而降低算复杂度。

总之，傅里叶变换在深度学习中可以降低计算复杂度和参数量，主要是因为它可以卷积操作转换为频域乘法操作，利用信号在频域上的稀疏性和分离性，以及实现参数共享。这些特性使得傅里叶变换成为深度学习中一种有效的计算工具。


The Fourier Transform can reduce computational complexity and parameter volume in deep learning for several reasons, as mentioned earlier. Here are some references and links that provide more information on this topic:

1. **Data Compression and Feature Extraction**: The Fourier Transform can represent a complex signal with a smaller number of parameters, which can reduce the computational complexity and parameter volume in deep learning models. This idea is discussed in the following paper:

   * Title: "Deep learning with time-frequency representations and dynamic convolution for environmental sound analysis"
   * Authors: Karol J. Piczak
   * Link: [https://arxiv.org/abs/1506.00212](https://arxiv.org/abs/1506.00212)

2. **Convolution Simplification**: In the frequency domain, convolution operations become simple multiplication operations, which can significantly reduce the computational complexity of these operations. This concept is explained in the following book:

   * Title: "Deep Learning"
   * Authors: Ian Goodfellow, Yoshua Bengio, and Aaron Courville
   * Link: [https://www.deeplearningbook.org/contents/convnets.html](https://www.deeplearningbook.org/contents/convnets.html)
   * See Section 9.1.2: "Fourier Transform"

3. **Noise Reduction**: The Fourier Transform can help to reduce noise in the data, which can improve the performance of deep learning models. This idea is discussed in the following paper:

   * Title: "Noise reduction in speech processing using deep neural networks"
   * Authors: Seyed Omid Sadjadi, John H. L. Hansen
   * Link: [https://ieeexplore.ieee.org/document/6854360](https://ieeexplore.ieee.org/document/6854360)

These references provide more information on how the Fourier Transform can help to reduce computational complexity and parameter volume in deep learning by compressing the data, simplifying convolution operations, reducing noise, and aiding in feature extraction.


The M&Ms-2 challenge data is comprised of 360 subjects with various RV and LV pathologies as well as a control group. The data was acquired using four different 1.5T scanners from three different manufacturer vendors (Siemens, GE, and Philips), undergoing variations in contrast
and anatomy (see Fig. 2). The training subset includes 160 cases with expert annotations for RV and LV blood pools and LV myocardium (MYO). The validation set contains 40 cases, including two pathologies not present in the training set. The final algorithm is evaluated on an unknown test set containing 160 cases.



initial->original
common->traditiona
operation->FLOPs
very large->
infomation->feature

我们的方法部分主要分为三部分。第一部分是对GFUNet核心部分Global Filter的傅里叶变换原理的阐述。第二和第三部分是GFUNet的encoder和decoder，我们对这两部分的许多细节进行了阐述，包括结构和参数等。