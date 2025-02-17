---
title: Evaluate for image fusion
---

[//]: # (  <span style="color: #589bd5; font-weight: bold;">Evaluate for Image Fusion</span>)
<h1 style="text-align: center;">
  <span style="color: #1B365D; font-weight: bold;">Evaluate for Image Fusion</span>
</h1>


***

## **Preamble**

图像融合是将来自不同来源的图像信息合成一幅图像的过程，目的是提高图像的质量和有效性。在图像融合研究中，评估融合结果的质量至关重要。为此，许多图像融合质量评估指标应运而生，帮助我们量化融合图像的优劣。

<span style="color: hsl(9, 100%, 60%);">这份笔记是笔者学习过程中所总结的，包含多类评估指标的定义以及实现代码，欢迎使用。</span>
<span style="color: #589bd5;  font-weight: bold;">
<a href="https://hinmouc.github.io/">
More in @hinmouc
</a>
</span>

***

## Overview

[//]: # ()
[//]: # (|         **Category**          |                                       **Metric Name**                                        |                   **Chinese**                   |                                              **Description**                                              |      **Direction**      |)

[//]: # (|:-----------------------------:|:--------------------------------------------------------------------------------------------:|:-----------------------------------------------:|:---------------------------------------------------------------------------------------------------------:|:-----------------------:|)

[//]: # (|  **Information Theory-Based**  |               **Entropy <span style="color: #589bd5;">&#40;EN&#41;</span>**               |                     **信息熵**                     |                                          衡量图像中包含的信息多少。                                          |       $\uparrow$        |)

[//]: # (|                               |                                 **Mutual Information &#40;MI&#41;**                                  |                     **互信息**                     |                                         衡量两幅图像共享信息的多少。                                         |       $\uparrow$        |)

[//]: # (|                               |                           **Normalized Mutual Information &#40;NMI&#41;**                            |                   **规范化互信息**                    |                                      互信息的标准化版本，消除尺度影响。                                      |       $\uparrow$        |)

[//]: # (|                               |                             **Feature Mutual Information &#40;FMI&#41;**                             |                    **特征互信息**                    |                          侧重于图像特征的互信息评估，帮助衡量图像中的特征信息共享。                          |       $\uparrow$        |)

[//]: # (|                               |                        **Quality Assessment Based on Fusion &#40;Qabf&#41;**                         |                  **基于梯度的融合性能**                  |                        评估融合图像中梯度信息的保持，主要衡量图像清晰度与细节保持。                        |       $\uparrow$        |)

[//]: # (|                               |                          **Noise-Based Fusion Performance &#40;Nabf&#41;**                           |                 **基于噪声评估的融合性能**                 |                       侧重于噪声对融合性能的影响，能够描述噪声抑制与保真度之间的平衡。                       |       $\downarrow$        |)

[//]: # (|                               |                    **Nonlinear Correlated Information Entropy &#40;Q_ncie&#41;**                     |                  **非线性相关信息熵**                   |                       结合非线性相关性与信息熵的混合评估方法，评估图像内容的复杂性。                        |       $\uparrow$        |)

[//]: # (|                               |                           **Phase Consistency-Based Metric &#40;Qp&#41;**                            |                **基于相位一致性的评价指标**                 |                         评估图像中各个部分的相位一致性，帮助描述图像细节的一致性。                         |       $\uparrow$        |)

[//]: # (|                               |                                                                                              |                                                 |                                                                                                             |                         |)

[//]: # (|    **Image Feature-Based**    |                                  **Average Gradient &#40;AG&#41;**                                   |                    **平均梯度**                     |                         衡量图像的清晰度和细节信息，通常用于评估图像的锐度和质量。                          |       $\uparrow$        |)

[//]: # (|                               |                                 **Standard Deviation &#40;SD&#41;**                                  |                     **标准差**                     |                           描述图像像素值的分散程度，反映图像的纹理信息和复杂度。                           |       $\uparrow$        |)

[//]: # (|                               |                                  **Spatial Frequency &#40;SF&#41;**                                  |                    **空间频率**                     |                           评估图像细节变化的频率，常用于评估图像的细节保留情况。                           |       $\uparrow$        |)

[//]: # (|                               |                                   **Edge Intensity &#40;EI&#41;**                                    |                    **边缘强度**                     |                             用于评估图像的边缘信息，反映边缘清晰度和细节表现。                             |       $\uparrow$        |)

[//]: # (|                               |                                                                                              |                                                 |                                                                                                             |                         |)

[//]: # (| **Structural Similarity-Based** |                            **Structural Similarity Index &#40;SSIM&#41;**                            |                   **结构相似性指数**                   |                                 评价图像的结构信息、对比度和亮度的相似性。                                 |       $\uparrow$        |)

[//]: # (|                               |                                **Multiscale SSIM &#40;MS-SSIM&#41;**                                 |                  **多尺度结构相似性**                   |                                      在多个尺度上评估图像的结构相似性。                                      |       $\uparrow$        |)

[//]: # (|                               |                             **Feature Similarity Index &#40;FSIM&#41;**                              |                   **特征相似性指数**                   |                                      基于图像的特征进行结构相似性评估。                                       |       $\uparrow$        |)

[//]: # (|                               |                                                                                              |                                                 |                                                                                                             |                         |)

[//]: # (|     **Correlation-Based**      |                               **Correlation Coefficient &#40;CC&#41;**                               |                    **相关系数**                     |                             评估图像之间的线性相关性，衡量它们之间的相关程度。                             |       $\uparrow$        |)

[//]: # (|                               |                       **Sum of the Correlations of Differences &#40;SCD&#41;**                       |                    **差异相关和**                    |                                         评估图像在频谱内容上的差异。                                         |       $\uparrow$        |)

[//]: # (|                               |                         **Nonlinear Correlation Coefficient &#40;NCC&#41;**                          |                   **非线性相关系数**                   |                      评估图像之间的非线性相关性，主要用于图像融合后的非线性相关度衡量。                      |       $\uparrow$        |)

[//]: # (|                               |                                                                                              |                                                 |                                                                                                             |                         |)

[//]: # (|   **Human Perception-Based**   |                            **Visual Information Fidelity &#40;VIF&#41;**                             |                   **视觉信息保真度**                   |                      从人类视觉系统角度评估图像质量，反映人眼感知的清晰度和信息保真度。                      |       $\uparrow$        |)

[//]: # (|                               |                              **Human Visual Perception &#40;QCB&#41;**                               |                   **人类视觉感知**                    |                             考虑人类视觉特性的图像质量评估，侧重图像的感知质量。                             |       $\uparrow$        |)

[//]: # (|                               |                                                                                              |                                                 |                                                                                                             |                         |)

[//]: # (|        **Error-Based**         |                                 **Mean Squared Error &#40;MSE&#41;**                                 |                    **均方误差**                     |                                      计算图像之间的平方差，评估误差大小。                                      |      $\downarrow$       |)

[//]: # (|                               |                            **Peak Signal-to-Noise Ratio &#40;PSNR&#41;**                             |                    **峰值信噪比**                    |                         基于MSE，评估图像质量的常用指标，信噪比越高，图像质量越好。                         |       $\uparrow$        |)


| **Category** | **Metric Name** |   **Chinese**    | **Description** | **Direction** |
|:---:|:---:|:----------------:|:---:|:---:|
| **Information Theory-Based** | **Entropy <span style="color: #589bd5;">(EN)</span>** |     **信息熵**      | 衡量图像中包含的信息多少。 | $\uparrow$ |
|  | **Mutual Information <span style="color: #589bd5;">(MI)</span>** |     **互信息**      | 衡量两幅图像共享信息的多少。 | $\uparrow$ |
|  | **Normalized Mutual Information <span style="color: #589bd5;">(NMI)</span>** |    **归一化互信息**    | 互信息的标准化版本，消除尺度影响。 | $\uparrow$ |
|  | **Feature Mutual Information <span style="color: #589bd5;">(FMI)</span>** |    **特征互信息**     | 侧重于图像特征的互信息评估，帮助衡量图像中的特征信息共享。 | $\uparrow$ |
|  | **Quality Assessment Based on Fusion <span style="color: #589bd5;">(Qabf)</span>** |  **基于梯度的融合性能**   | 评估融合图像中梯度信息的保持，主要衡量图像清晰度与细节保持。 | $\uparrow$ |
|  | **Noise-Based Fusion Performance <span style="color: #589bd5;">(Nabf)</span>** | **基于噪声评估的融合性能**  | 侧重于噪声对融合性能的影响，能够描述噪声抑制与保真度之间的平衡。 | $\downarrow$ |
|  | **Nonlinear Correlated Information Entropy <span style="color: #589bd5;">(Q_ncie)</span>** |   **非线性相关信息熵**   | 结合非线性相关性与信息熵的混合评估方法，评估图像内容的复杂性。 | $\uparrow$ |
|  | **Phase Consistency-Based Metric <span style="color: #589bd5;">(Qp)</span>** | **基于相位一致性的评价指标** | 评估图像中各个部分的相位一致性，帮助描述图像细节的一致性。 | $\uparrow$ |
|  |  |                  |  |  |
| **Image Feature-Based** | **Average Gradient <span style="color: #589bd5;">(AG)</span>** |     **平均梯度**     | 衡量图像的清晰度和细节信息，通常用于评估图像的锐度和质量。 | $\uparrow$ |
|  | **Standard Deviation <span style="color: #589bd5;">(SD)</span>** |     **标准差**      | 描述图像像素值的分散程度，反映图像的纹理信息和复杂度。 | $\uparrow$ |
|  | **Spatial Frequency <span style="color: #589bd5;">(SF)</span>** |     **空间频率**     | 评估图像细节变化的频率，常用于评估图像的细节保留情况。 | $\uparrow$ |
|  | **Edge Intensity <span style="color: #589bd5;">(EI)</span>** |     **边缘强度**     | 用于评估图像的边缘信息，反映边缘清晰度和细节表现。 | $\uparrow$ |
|  |  |                  |  |  |
| **Structural Similarity-Based** | **Structural Similarity Index <span style="color: #589bd5;">(SSIM)</span>** |   **结构相似性指数**    | 评价图像的结构信息、对比度和亮度的相似性。 | $\uparrow$ |
|  | **Multiscale SSIM <span style="color: #589bd5;">(MS-SSIM)</span>** |   **多尺度结构相似性**   | 在多个尺度上评估图像的结构相似性。 | $\uparrow$ |
|  | **Feature Similarity Index <span style="color: #589bd5;">(FSIM)</span>** |   **特征相似性指数**    | 基于图像的特征进行结构相似性评估。 | $\uparrow$ |
|  |  |                  |  |  |
| **Correlation-Based** | **Correlation Coefficient <span style="color: #589bd5;">(CC)</span>** |     **相关系数**     | 评估图像之间的线性相关性，衡量它们之间的相关程度。 | $\uparrow$ |
|  | **Sum of the Correlations of Differences <span style="color: #589bd5;">(SCD)</span>** |    **差异相关和**     | 评估图像在频谱内容上的差异。 | $\uparrow$ |
|  | **Nonlinear Correlation Coefficient <span style="color: #589bd5;">(NCC)</span>** |   **非线性相关系数**    | 评估图像之间的非线性相关性，主要用于图像融合后的非线性相关度衡量。 | $\uparrow$ |
|  |  |                  |  |  |
| **Human Perception-Based** | **Visual Information Fidelity <span style="color: #589bd5;">(VIF)</span>** |   **视觉信息保真度**    | 从人类视觉系统角度评估图像质量，反映人眼感知的清晰度和信息保真度。 | $\uparrow$ |
|  | **Human Visual Perception <span style="color: #589bd5;">(QCB)</span>** |    **人类视觉感知**    | 考虑人类视觉特性的图像质量评估，侧重图像的感知质量。 | $\uparrow$ |
|  |  |                  |  |  |
| **Error-Based** | **Mean Squared Error <span style="color: #589bd5;">(MSE)</span>** |     **均方误差**     | 计算图像之间的平方差，评估误差大小。 | $\downarrow$ |
|  | **Peak Signal-to-Noise Ratio <span style="color: #589bd5;">(PSNR)</span>** |    **峰值信噪比**     | 基于MSE，评估图像质量的常用指标，信噪比越高，图像质量越好。 | $\uparrow$ |

---


<h1 style="text-align: center;">
  <span style="color: #eb7d2e; ">Detailed description</span>
</h1>

<span style="font-weight: bold;">Note: 下列代码所用到的check函数，如下所示</span>
```python
    @classmethod
    def input_check(cls, imgF, imgA=None, imgB=None): 
        if imgA is None:
            assert type(imgF) == np.ndarray, 'type error'
            assert len(imgF.shape) == 2, 'dimension error'
        else:
            assert type(imgF) == type(imgA) == type(imgB) == np.ndarray, 'type error'
            assert imgF.shape == imgA.shape == imgB.shape, 'shape error'
            assert len(imgF.shape) == 2, 'dimension error'
```


***


## 1.Information Theory-Based

### (1) Entropy <span style="color: #589bd5; font-weight: bold;">(EN)</span>
* **Chinese**：**信息熵**
* **Calculation Object**: **Single Image**
* **Evaluation Direction**：<span style="color: #d6292a;  font-weight: bold;">越大越好</span>
* **Description**：信息熵用于量化图像的信息含量。在大多数应用中，较高的信息熵意味着图像包含更多的信息和细节。在图像融合中，高熵的融合结果表明图像综合了更多的源信息。

---

#### **Formulas**：	

$$
H(i) = -\sum_{i=0}^{i-1} p(i) \log_2 p(i)
$$

**element**：

* $H(i)$：信息熵，衡量图像信息的不确定性或复杂性。
* $p(i)$：像素值 $i$ 在图像中出现的概率，计算为像素值 $i$ 的频数除以图像中的总像素数。
* $\log_2 p(i)$：以2为底的对数函数，用于计算以比特为单位的信息量。
* $\sum_{i}$：求和符号，表示对所有可能的像素值 $i$ 进行累加。


***


#### **Code：**

```python
    @classmethod
    def EN(cls, img):  # entropy
        cls.input_check(img)
        a = np.uint8(np.round(img)).flatten()
        h = np.bincount(a) / a.shape[0]
        return -sum(h * np.log2(h + (h == 0)))
```


---


### (2) Mutual Information <span style="color: #589bd5;">(MI)</span>

* **Chinese**：**互信息**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：互信息用于衡量两幅图像之间共享的信息量。互信息越大，表示两幅图像之间的重叠信息越多。例如，在图像融合中，较高的互信息值意味着融合图像综合了更多源图像的信息。

* * *

#### **Formulas**：

$$
MI = MI_{A,F} + MI_{B,F}
$$

其中$MI_{X,F}$的表达式如下（X代表图像A或图像B）。融合图像的 MI 越高,意味着相应的融合算法从源图像中转移到融合图像中的信息越多，公式如下：

$$
MI_{X,F} = \sum_{x,f} P_{X,F}(x,f) \log \frac{P_{X,F}(x,f)}{P_X(x) P_F(f)}
$$

**element**：

* $MI_{X,F}$：两幅图像 $X$ 和 $F$ 之间的互信息。
* $P_{X,F}(x,f)$：联合概率分布，表示图像 $X$ 中像素值为 $x$ 和图像 $F$ 中像素值为 $f$ 同时出现的概率。
* $P_X(x)$ 和 $P_F(f)$：图像 $X$ 和 $F$ 的边缘概率分布。


***


#### **Code**：

```python
    @classmethod
    def MI(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        return skm.mutual_info_score(image_F.flatten(), image_A.flatten()) +
            skm.mutual_info_score(image_F.flatten(), image_B.flatten())
```

* * *

### (3) Normalized Mutual Information <span style="color: #589bd5;">(NMI)</span>

* **Chinese**：**归一化互信息**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：归一化互信息（NMI）是互信息（MI）的标准化版本，用于度量两幅图像之间共享的信息量，相对于每幅图像自身的信息量。NMI的值范围通常在0到1之间，其中较高的NMI值意味着两幅图像之间的信息共享较多，特别适用于图像配准等任务。

* * *

#### **Formulas**：

$$
\text{NMI}(X,Y) = \frac{2 \times \text{MI}(X,Y)}{H(X) + H(Y)}
$$

**element**：

* $\text{NMI}(X, Y)$：归一化互信息，衡量两幅图像 $X$ 和 $Y$ 之间的标准化信息共享量。
* $\text{MI}(X, Y)$：互信息，衡量两幅图像 $X$ 和 $Y$ 之间的共享信息量，公式见上文介绍。
* $H(X)$：图像 $X$ 的信息熵，衡量图像 $X$ 的信息量。
* $H(Y)$：图像 $Y$ 的信息熵，衡量图像 $Y$ 的信息量。
* $\frac{2 \times \text{MI}(X,Y)}{H(X) + H(Y)}$：通过互信息与信息熵的比值来标准化信息共享量，避免尺度差异的影响。


***


#### **Code**：

```python
    @classmethod
    def calculate_NMI(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)

        def calc_entropy(img):
            hist = np.histogram(img, bins=256)[0]
            hist = hist / hist.sum()
            hist = hist[hist > 0]  # Remove zeros to avoid log(0)
            return -np.sum(hist * np.log2(hist))

        # Calculate entropy for each image
        H_F = calc_entropy(image_F)
        H_A = calc_entropy(image_A)
        H_B = calc_entropy(image_B)

        # Calculate mutual information between fused image and each source image
        MI_FA = mutual_info_score(None, None, contingency=np.histogram2d(image_F.ravel(), image_A.ravel(), bins=256)[0])
        MI_FB = mutual_info_score(None, None, contingency=np.histogram2d(image_F.ravel(), image_B.ravel(), bins=256)[0])

        # Calculate NMI for each pair
        NMI_FA = MI_FA / np.sqrt(H_F * H_A)
        NMI_FB = MI_FB / np.sqrt(H_F * H_B)

        # Return average NMI
        return (NMI_FA + NMI_FB) / 2
```

***


### (4) Feature Mutual Information <span style="color: #589bd5;">(FMI)</span>

* **Chinese**：**特征互信息**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：特征互信息（FMI）是互信息的一种变体，专注于图像的频率域特征。它常用于评估图像融合的效果。FMI越高，表示更多的特征信息从源图像转移到融合图像中，尤其在图像融合与特征提取的任务中具有重要意义。

* * *

#### **Formulas**：

**First Formula**:

$$
FMI = MI_{A,F} + MI_{B,F}
$$

**Second Formula**:

$$
\text{FMI} = \sum \frac{|\text{FFT}(I_1) \cdot \text{conj}(\text{FFT}(I_2))|}{|\text{FFT}(I_1)| \cdot |\text{FFT}(I_2)|}
$$

**element**：

* $MI_{A,F}$：源图像 $A$ 与融合图像 $F$ 之间的互信息。
* $MI_{B,F}$：源图像 $B$ 与融合图像 $F$ 之间的互信息。
* $\text{FFT}(I_1)$、$\text{FFT}(I_2)$：对图像 $I_1$ 和 $I_2$ 进行快速傅里叶变换（FFT），将其从空间域转换到频率域。
* $\text{conj}(\text{FFT}(I_2))$：对图像 $I_2$ 的FFT结果进行共轭操作。
* $\sum$：求和符号，表示对所有频率分量进行累加。


***


#### **Code**：

```python
    @classmethod
    def calculate_FMI(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)

        def mutual_info(img1, img2):
            hist_2d, _, _ = np.histogram2d(img1.ravel(), img2.ravel(), bins=256)
            pxy = hist_2d / float(np.sum(hist_2d))
            px = np.sum(pxy, axis=1)  # marginal for x over y
            py = np.sum(pxy, axis=0)  # marginal for y over x
            px_py = px[:, None] * py[None, :]  # Broadcast to multiply marginals
            # Now we can do the calculation using the pxy, px_py 2D arrays
            nzs = pxy > 0  # Only non-zero pxy values contribute to the sum
            return np.sum(pxy[nzs] * np.log(pxy[nzs] / px_py[nzs]))

        # Compute edges as features
        edges_F = feature.canny(image_F)
        edges_A = feature.canny(image_A)
        edges_B = feature.canny(image_B)

        FMI_A = mutual_info(edges_F, edges_A)
        FMI_B = mutual_info(edges_F, edges_B)

        # Here, we simply average the FMI values of two comparisons
        FMI = (FMI_A + FMI_B) / 2
        return FMI
```

***

### (5) Quality Assessment Based on Fusion <span style="color: #589bd5;">(Qabf)</span>

* **Chinese**：**基于融合的质量评估**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：Qabf是一种新颖的融合图像客观非参考质量评估指标，旨在估计图像在融合后表现出的显著信息。该指标通过局部度量来估计每个输入图像信息在融合图像中的表现，值越高表示融合图像质量越好，通常用于图像融合任务中。

* * *

#### **Formulas**：

$$
\mathrm{Q(a,b,f)~=~\frac{1}{|W|}\sum_{\omega\in W}\left(\lambda(\omega)Q_0\left(a,f|\omega\right)+\left(1-\lambda(\omega)\right)Q_0\left(b,f|\omega\right)\right)}
$$

**element**：

* $Q_0(a,f|\omega)$：在局部窗口 $\omega$ 下，源图像 $a$ 与融合图像 $f$ 的质量评估。
* $\lambda(\omega)$：与窗口 $\omega$ 相关的加权系数，反映图像局部信息的重要性。
* $W$：图像的所有局部窗口集合，表示图像在空间上的分块。
* $|W|$：窗口的总数，通常表示图像分块的数量。


***


#### **Code**：

```python
    @classmethod
    def Qabf(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        gA, aA = cls.Qabf_getArray(image_A)
        gB, aB = cls.Qabf_getArray(image_B)
        gF, aF = cls.Qabf_getArray(image_F)
        QAF = cls.Qabf_getQabf(aA, gA, aF, gF)
        QBF = cls.Qabf_getQabf(aB, gB, aF, gF)

    # 计算QABF
     deno = np.sum(gA + gB)
     nume = np.sum(np.multiply(QAF, gA) + np.multiply(QBF, gB))
     return nume / deno

    @classmethod
    def Qabf_getArray(cls,img):
    
        # Sobel Operator Sobel
    
         h1 = np.array([[1, 2, 1], [0, 0, 0], [-1, -2, -1]]).astype(np.float32)
         h2 = np.array([[0, 1, 2], [-1, 0, 1], [-2, -1, 0]]).astype(np.float32)
         h3 = np.array([[-1, 0, 1], [-2, 0, 2], [-1, 0, 1]]).astype(np.float32)
    
         SAx = convolve2d(img, h3, mode='same')
         SAy = convolve2d(img, h1, mode='same')
         gA = np.sqrt(np.multiply(SAx, SAx) + np.multiply(SAy, SAy))
         aA = np.zeros_like(img)
         aA[SAx == 0] = math.pi / 2
         aA[SAx != 0]=np.arctan(SAy[SAx != 0] / SAx[SAx != 0])
         return gA, aA
    
    @classmethod
    def Qabf_getQabf(cls,aA, gA, aF, gF):
        L = 1
        Tg = 0.9994
        kg = -15
        Dg = 0.5
        Ta = 0.9879
        ka = -22
        Da = 0.8
        GAF,AAF,QgAF,QaAF,QAF = np.zeros_like(aA),np.zeros_like(aA),np.zeros_like(aA),np.zeros_like(aA),np.zeros_like(aA)
        GAF[gA>gF]=gF[gA>gF]/gA[gA>gF]
        GAF[gA == gF] = gF[gA == gF]
        GAF[gA <gF] = gA[gA<gF]/gF[gA<gF]
        AAF = 1 - np.abs(aA - aF) / (math.pi / 2)
        QgAF = Tg / (1 + np.exp(kg * (GAF - Dg)))
        QaAF = Ta / (1 + np.exp(ka * (AAF - Da)))
        QAF = QgAF* QaAF
        return QAF
```

***


### (6) Noise-Based Fusion Performance <span style="color: #589bd5;">(Nabf)</span>

* **Chinese**：**基于伪影的指标**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越小越好</span>
* **Description**：基于伪影的指标（$N^{AB/F}$）是一种评估图像融合质量的标准，它通过计算图像中伪影位置的影响，来衡量噪声对融合图像的影响。该指标的值越小，表示融合图像中噪声和伪影的影响越小，质量越高。

* * *

#### **Formulas**：

$$
N^{AB/F} = \frac{\sum_{i=1}^{M}\sum_{j=1}^{N}AM( i,j)\left[ ( 1 - Q^{A,F}( i,j) ) w^{A}( i,j) + ( 1 - Q^{B,F}( i,j) ) w^{B}( i,j) \right]}{\sum_{i=1}^{M} \sum_{j=1}^{N} ( w^{A}( i,j) + w^{n}( i,j) )}
$$

$$
AM(i,j) =  
    \begin{cases}  
    1 & \text{if } O^f_g(i,j) > O^x_g(i,j) \\  
    0 & \text{otherwise}  
    \end{cases}
$$


**element**：

* $N^{AB/F}$：基于噪声的融合性能指标。
* $AM(i,j)$：位置 (i,j) 在参考图像A和B的平均值。
* $Q^{A,F}(i,j)$ 和 $Q^{B,F}(i,j)$：位置 (i,j) 在融合图像和参考图像A、B之间的质量评估指标。
* $w^{A}(i,j)$ 和 $w^{B}(i,j)$：在位置 (i,j) 的权重因子，反映该位置的噪声敏感度。
* $\sum$：求和操作，用于累加整个图像区域。
* AM(i,j)表示图像伪影位置，即如果融合图像中某处的边缘强度比源图像中相应位置处的都强,则判定为伪影


***


#### **Code**：

```python
    @classmethod
    def estimate_noise(cls, I):
        H, W = I.shape
        M = [[1, -2, 1], [-2, 4, -2], [1, -2, 1]]
        sigma = np.sum(np.sum(np.abs(convolve2d(I, M, mode='same'))))
        return sigma / (H * W)

    @classmethod
    def Nabf(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        noise_F = cls.estimate_noise(image_F)
        noise_A = cls.estimate_noise(image_A)
        noise_B = cls.estimate_noise(image_B)

        if noise_A + noise_B == 0:
            return float('inf')  # 避免除以零
        Nabf = (noise_A + noise_B - 2 * noise_F) / (noise_A + noise_B)
        return Nabf
```

***

### (7) Nonlinear Correlated Information Entropy <span style="color: #589bd5;">(Q_ncie)</span>

* **Chinese**：**非线性相关信息熵**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：非线性相关信息熵 (Q_ncie) 用于评估图像间非线性关系的保持程度。它通过计算图像的联合概率分布来衡量图像之间非线性相关性的信息量。在医学成像等领域，该指标能够评估图像是否有效地保留了源图像中的非线性特征。

* * *

#### **Formulas**：

$$
Q_{ncie} = \sum_{x,f} \left( \frac{p(x,f)}{p(x)p(f)} \right) \log \left( \frac{p(x,f)}{p(x)p(f)} \right)
$$

**element**：

* $Q_{ncie}$：非线性相关信息熵。
* $p(x,f)$：联合概率密度函数，表示两个图像像素值x和f同时出现的概率。
* $p(x)$ 和 $p(f)$：两个图像的边缘概率密度函数。
* $\log$：自然对数，用于计算信息熵。


***


#### **Code**：

```python
    @classmethod
    def calculate_Q_ncie(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)

        # 计算图像间的非线性相关信息熵
        def calc_Q_ncie(img1, img2):
            joint_hist, _, _ = np.histogram2d(img1.ravel(), img2.ravel(), bins=256, density=True)
            pxy = joint_hist / np.sum(joint_hist)  # 联合概率
            px = np.sum(pxy, axis=1)  # 边缘概率
            py = np.sum(pxy, axis=0)
            pxy_nz = pxy[pxy > 0]
            px_py = np.outer(px, py)[pxy > 0]
            Q_ncie = np.sum(pxy_nz * np.log(pxy_nz / px_py))
            return Q_ncie

        # 计算融合图像与两个源图像的Q_ncie并取平均
        Q_ncie_FA = calc_Q_ncie(image_F, image_A)
        Q_ncie_FB = calc_Q_ncie(image_F, image_B)
        return (Q_ncie_FA + Q_ncie_FB) / 2
```

***


### (8) Phase Congruency Evaluation <span style="color: #589bd5;">(Qp)</span>

* **Chinese**：**基于相位一致性的评价指标**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：基于相位一致性的评价指标 (Qp) 专注于评估图像中的相位一致性，反映了图像的边缘和特征的显著性和锐利度。相位一致性高的图像通常意味着图像的特征更加明显，对于边缘检测和图像融合等任务尤其重要。

* * *

#### **Formulas**：

$$
Q_p = \frac{1}{MN} \sum_{i=0}^{M-1} \sum_{j=0}^{N-1} PC_A(i,j) \cdot PC_F(i,j)
$$

**element**：

* $Q_p$：基于相位一致性的评价指标。
* $PC_A(i,j)$ 和 $PC_F(i,j)$：分别是参考图像A和融合图像在位置(i, j)的相位一致性。
* $M, N$：图像的宽度和高度。
* $\sum$：求和操作，用于累加所有像素点。


***


#### **Code**：

```python
    @classmethod
    def calculate_Qp(cls, image_F, image_A, image_B):
        from skimage.feature import canny
        from skimage import img_as_float
        cls.input_check(image_F, image_A, image_B)

        # 使用Canny算子作为相位一致性的近似计算
        def calc_phase_congruency(img1, img2):
            pc1 = canny(img_as_float(img1))
            pc2 = canny(img_as_float(img2))
            return np.sum(pc1 == pc2) / np.sum(pc1 | pc2)  # 计算相位一致性指数

        # 计算融合图像与两个源图像的相位一致性指数并取平均
        Qp_FA = calc_phase_congruency(image_F, image_A)
        Qp_FB = calc_phase_congruency(image_F, image_B)
        return (Qp_FA + Qp_FB) / 2
```



---



## 2.Image Feature-Based

### (1) Standard Deviation <span style="color: #589bd5;">(SD)</span>

* **Chinese**：**标准差**
* **Calculation Object**: **Single Image**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：标准差反映了图像像素值的变化范围，常用来评估图像的对比度、清晰度等。较高的标准差通常意味着图像具有较高的对比度和更清晰的细节，但过高的标准差可能导致噪声或失真。对于融合图像，SD越大，图像细节可能越丰富，但也需要平衡噪声和清晰度之间的关系。
* **具体来说，SD的大小与融合图像的对比度、清晰度和一致性程度存在一定的关联，所以也并不是越大就一定越好。**

  1. 对比度： SD的大小与融合图像的对比度有一定的正相关关系，即SD越大，对比度可能也会越高。
       但是，过高的对比度可能导致图像过亮或过暗，影响观察体验。

  2. 清晰度： SD的大小与融合图像的清晰度也有一定的正相关关系，即SD越大，图像细节可能也会更加清晰。
     但是，过高的SD可能导致图像出现过多噪声或失真，影响观察体验。

  3. 一致性： SD的大小与融合图像的一致性程度没有明确的相关关系，因此SD的大小并不能直接反映融合图像的一致性。

* * *

#### **Formulas**：

$$
SD = \sqrt{\sum_{i=1}^M \sum_{j=1}^N (F(i,j) - \mu)^2}
$$

**element**：

* $SD$：标准差，衡量像素值相对于平均值的变异程度。
* $F(i,j)$：图像在位置 $(i, j)$ 的像素值。
* $\mu$：图像的平均像素值。
* $\sum_{i=1}^M \sum_{j=1}^N$：求和符号，表示对所有像素值进行累加。


***


#### **Code**：

```python
    @classmethod
    def SD(cls, img):
        cls.input_check(img)
        return np.std(img)
```



---



### (2) Spatial Frequency <span style="color: #589bd5;">(SF)</span>

* **Chinese**：**空间频率**
* **Calculation Object**: **Single Image**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：空间频率反映了图像灰度值变化的速度，通常用于衡量图像的清晰度。较高的空间频率意味着图像具有更多的边缘和细节，融合图像的质量通常越高。
SF通过计算图像的梯度变化来揭示图像的纹理和边缘信息，是图像清晰度和细节的有效指标。

* * *

#### **Formulas**：

$$
\mathrm{SF}\:=\:\sqrt{\mathrm{RF}^2+\mathrm{CF}^2}
$$

其中，

$$
\mathrm{RF~=~\sqrt{\frac{1}{MN}\sum_{i=1}^M\sum_{j=1}^N\left(H(i,j)-H(i,j-1)\right
)^2}}
$$

$$
\mathrm{CF~=~\sqrt{\frac{1}{MN}\sum_{i=1}^M\sum_{j=1}^N\left(H(i,j)-H(i-1,j)\right)^2}}
$$

**element**：

* $RF$：表示行频率。
* $CF$：表示列频率。


***


#### **Code**：

```python
    @classmethod
    def SF(cls, img):
        cls.input_check(img)
        return np.sqrt(np.mean((img[:, 1:] - img[:, :-1]) ** 2) + np.mean((img[1:, :] - img[:-1, :]) ** 2))
```



---



### (3) Average Gradient <span style="color: #589bd5;">(AG)</span>

* **Chinese**：**平均梯度**
* **Calculation Object**: **Single Image**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：平均梯度用于衡量图像的清晰度，它是通过计算图像中相邻像素之间的梯度来评估图像的锐利程度。在图像融合中，较高的平均梯度值通常意味着图像包含更多细节，并且融合质量较高。

* * *

#### **Formulas**：

$$
\mathrm{AG~=~\frac{1}{(M-1)(N-1)}\sum_{i=1}^{M-1}\sum_{i=1}^{N-1}\sqrt{\frac{\left(H(i+1,j)-H(i,j)\right)^2+\left(H(i,j+1)-H(i,j)\right)^2}{2}}}
$$

**element**：

* $AG$：平均梯度，衡量图像中像素变化的平均程度，反映了图像的清晰度。
* $H(i,j)$：融合图像在位置 $(i,j)$ 的像素值。
* $M$ 和 $N$：图像的高度和宽度，分别表示图像的行数和列数。
* $Gx$ 和 $Gy$：图像在水平方向和垂直方向的梯度。


***


#### **Code**：

```python
    @classmethod
    def AG(cls, img):  # Average gradient
        cls.input_check(img)
        Gx, Gy = np.zeros_like(img), np.zeros_like(img)
        Gx[:, 0] = img[:, 1] - img[:, 0]
        Gx[:, -1] = img[:, -1] - img[:, -2]
        Gx[:, 1:-1] = (img[:, 2:] - img[:, :-2]) / 2
    
        Gy[0, :] = img[1, :] - img[0, :]
        Gy[-1, :] = img[-1, :] - img[-2, :]
        Gy[1:-1, :] = (img[2:, :] - img[:-2, :]) / 2
        return np.mean(np.sqrt((Gx ** 2 + Gy ** 2) / 2))
```

***

### (4) Edge Intensity <span style="color: #589bd5;">(EI)</span>

* **Chinese**：**边缘强度**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越小越好</span>
* **Description**：边缘强度（EI）是用来评估图像中边缘的清晰度和强度的一个指标。边缘通常代表了图像中显著的结构信息，对于图像融合来说，保留原始图像中的边缘信息是确保融合质量的重要因素。

* * *

#### **Formulas**：

$$
EI = \frac{1}{MN} \sum_{i=0}^{M-1} \sum_{j=0}^{N-1} |E_F(i,j) - E_A(i,j)|
$$

**element**：

* $EI$：边缘强度指标。
* $E_F(i,j)$：融合图像在位置 (i,j) 的边缘强度。
* $E_A(i,j)$：参考图像A在位置 (i,j) 的边缘强度。
* $M, N$：图像的宽度和高度。
* $\sum$：求和操作，用于累加所有像素点。


***


#### **Code**：

```python
    @classmethod
    def calculate_avg_EI(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        ei_A = np.mean(np.abs(filters.sobel(image_F) - filters.sobel(image_A)))
        ei_B = np.mean(np.abs(filters.sobel(image_F) - filters.sobel(image_B)))
        avg_EI = (ei_A + ei_B) / 2
        return avg_EI
```


---



## 4.Structural Similarity-Based

### (1) Structural Similarity Index Measure <span style="color: #589bd5;">(SSIM)</span>

* **Chinese**：**结构相似性**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越接近1越好</span>
* **Description**：SSIM（结构相似性指数）用于衡量两幅图像的相似度，考虑了亮度、对比度和结构三个方面的信息。SSIM 值的范围为[-1, 1]，值越接近 1 表示两幅图像越相似，越接近 -1 表示图像越不相似，值为 0 表示图像完全不同。它主要用于评估图像融合结果的质量。

* * *

#### **Formulas**：
$$
\text{SSIM} = \frac{SSIM_{A,F}+SSIM_{B,F}}2
$$

其中,

$$
SSIM_{X,F} = \sum_{x,f} \left(\frac{2 \mu_x \mu_f + C_1}{\mu_x^2 + \mu_f^2 + C_1} \times \frac{2 \sigma_{xf} + C_2}{\sigma_x^2 + \sigma_f^2 + C_2} \times \frac{\sigma_{xf} + C_3}{\sigma_x \sigma_f + C_3}\right)
$$

**element:**

* $\mu_x, \mu_f$：图像 X 和 F 的局部均值。
* $\sigma_x, \sigma_f$：图像 X 和 F 的局部标准差。
* $\sigma_{xf}$：X 和 F 的局部协方差。
* $C_1, C_2, C_3$：为了避免分母为零而添加的小常数。代码如下：（越接近1越好，输入三幅图像）


***


#### **Code**：

```python
   from skimage.metrics import structural_similarity as ssim
    @classmethod
    def SSIM(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        return ssim(image_F,image_A)+ssim(image_F,image_B)
```



---


### (1) Multiscale Structural Similarity Index <span style="color: #589bd5;">(MS-SSIM)</span>

* **Chinese**：**多尺度结构相似性**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越接近1越好</span>
* **Description**：多尺度结构相似性指数（MS-SSIM）是对传统SSIM的一种扩展，通过在多个尺度上评估图像的结构相似性，提供了更为精确的质量评估方法。它模拟了人眼对图像质量的感知，采用多级下采样，在每个尺度上计算SSIM值，并通过加权方式结合这些信息，以反映图像质量的多尺度感知。

* * *

#### **Formulas**：

$$
MS\text{-}SSIM(x, f) = [L_M(x, f)]^{\alpha_M} \times \prod_{j=1}^{M} [c_j(x, f)]^{\beta_j} \times [s_j(x, f)]^{\gamma_j}
$$

**element**:

* $L_M(x, f)$：在 M 尺度上的亮度比较。
* $c_j(x, f)$：在第 j 尺度上的对比度比较。
* $s_j(x, f)$：在第 j 尺度上的结构比较。
* $\alpha_M, \beta_j, \gamma_j$：是加权指数，用于调整不同尺度、亮度、对比度和结构比较的重要性。


***


#### **Code**：

MS-SSIM的计算一般较为复杂，需要在多个尺度上进行图像处理。由于 `skimage` 的 `ssim` 计算方法并不支持多尺度直接计算，我们通常需要使用外部库或者自己实现多尺度的过程。以下是一个大致的实现框架：

```python
    from skimage.metrics import structural_similarity as ssim
    import numpy as np
    
    def calculate_ms_ssim(image_F, image_A, image_B, max_scale=5):
        def downsample_image(image):
            # 用于图像下采样的简单函数，可以使用不同的方法
            return image[::2, ::2]  # 这里用的是2x下采样，实际可以用更多下采样方法
    
        def compute_single_scale_ssim(image1, image2):
            return ssim(image1, image2)
    
        ms_ssim_A, ms_ssim_B = 1.0, 1.0
        for scale in range(max_scale):
            image_A_downsampled = downsample_image(image_A)
            image_F_downsampled = downsample_image(image_F)
            image_B_downsampled = downsample_image(image_B)
            
            # 计算每个尺度上的SSIM
            scale_ssim_A = compute_single_scale_ssim(image_A_downsampled, image_F_downsampled)
            scale_ssim_B = compute_single_scale_ssim(image_B_downsampled, image_F_downsampled)
    
            # 结合每个尺度的结果
            ms_ssim_A *= scale_ssim_A
            ms_ssim_B *= scale_ssim_B
    
            # 下采样后的图像继续作为下一尺度的输入
            image_A = image_A_downsampled
            image_F = image_F_downsampled
            image_B = image_B_downsampled
    
        # 最终MS-SSIM为所有尺度结果的乘积
        ms_ssim = (ms_ssim_A + ms_ssim_B) / 2
        return ms_ssim
```

* * *


### (3) Feature Similarity Index <span style="color: #589bd5;">(FSIM)</span>

* **Chinese**：**特征相似性指数**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越接近1越好</span>
* **Description**：特征相似性指数（FSIM）是一种通过对比图像的局部特征来衡量图像融合效果的质量评估指标。该指标特别关注图像的结构特征，通过计算图像在各个位置的均值、标准差及它们的关系来量化融合图像与参考图像的相似度。

* * *

#### **Formulas**：

$$
FSIM = \frac{1}{MN} \sum_{i=0}^{M-1} \sum_{j=0}^{N-1} \left( \frac{2 \mu_A(i,j) \mu_F(i,j) + C_1}{\mu_A^2(i,j) + \mu_F^2(i,j) + C_1} \right) \times \left( \frac{2 \sigma_{AF}(i,j) + C_2}{\sigma_A(i,j) + \sigma_F(i,j) + C_2} \right)
$$

**element**：

* $FSIM$：特征相似性指数。
* $\mu_A(i,j)$ 和 $\mu_F(i,j)$：在位置 (i,j) 的参考图像A和融合图像的局部均值。
* $\sigma_A(i,j)$ 和 $\sigma_F(i,j)$：在位置 (i,j) 的参考图像A和融合图像的局部标准差。
* $C_1, C_2$：为了避免除零错误而添加的小常数。
* $M, N$：图像的宽度和高度。
* $\sum$：求和操作，用于累加所有像素点。

**算法描述**： FSIM通过计算图像的局部均值和标准差来评估图像的特征相似性。对于每个像素点，FSIM考虑了该点周围的均值和标准差，以反映图像的结构特征。然后，这些特征信息在图像的所有像素点上进行累加，从而得到融合图像与参考图像之间的整体相似性。FSIM特别关注结构信息，因此对图像的细节保留非常敏感。


***


#### **Code**：

```python
import numpy as np

class ImageMetrics:
    @classmethod
    def FSIM(cls, image_F, image_A, image_B):
        """
        计算特征相似性指数 (FSIM)。
        :param image_F: 融合图像
        :param image_A: 参考图像 A
        :param image_B: 参考图像 B
        :return: FSIM 值
        """
        cls.input_check(image_F, image_A, image_B)

        def compute_fsim(img1, img2):
            """
            计算图像之间的 FSIM 值。
            :param img1: 融合图像
            :param img2: 参考图像
            :return: FSIM 值
            """
            # 计算局部均值和标准差
            mu_img1 = cv2.GaussianBlur(img1, (3, 3), 0)
            mu_img2 = cv2.GaussianBlur(img2, (3, 3), 0)
            sigma_img1 = cv2.GaussianBlur(img1 ** 2, (3, 3), 0) - mu_img1 ** 2
            sigma_img2 = cv2.GaussianBlur(img2 ** 2, (3, 3), 0) - mu_img2 ** 2
            sigma_img1 = np.sqrt(sigma_img1)
            sigma_img2 = np.sqrt(sigma_img2)

            # 计算特征相似性
            numerator = 2 * mu_img1 * mu_img2 + 1e-6
            denominator = mu_img1 ** 2 + mu_img2 ** 2 + 1e-6
            similarity = numerator / denominator

            numerator_sigma = 2 * sigma_img1 * sigma_img2 + 1e-6
            denominator_sigma = sigma_img1 ** 2 + sigma_img2 ** 2 + 1e-6
            similarity_sigma = numerator_sigma / denominator_sigma

            fsim_map = similarity * similarity_sigma
            return np.mean(fsim_map)

        # 计算两个参考图像与融合图像的 FSIM
        fsim_A = compute_fsim(image_F, image_A)
        fsim_B = compute_fsim(image_F, image_B)
        
        # 取平均值
        return (fsim_A + fsim_B) / 2
```


* * *




## 4.Correlation-Based

### (1) Correlation Coefficient <span style="color: #589bd5;">(CC)</span>

* **Chinese**：**相关系数**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越接近1越好</span>
* **Description**：相关系数（CC）用于衡量两幅图像之间的线性相关性。它常用于评估图像的相似性，特别是在图像融合中，用于评估融合图像与源图像之间的相似度。相关系数越高，表示融合图像与源图像越相似。

* * *

#### **Formulas**：

$$
CC = \frac{r(A,F) + r(B,F)}{2}
$$

$$
r(X,F) = \frac{\sum_{i=1}^M \sum_{j=1}^N (X(i,j) - \bar{X})(F(i,j) - \mu)}{\sqrt{\sum_{i=1}^M \sum_{j=1}^N (X(i,j) - \bar{X})^2 \sum_{i=1}^M \sum_{j=1}^N (F(i,j) - \mu)^2}}
$$

**element**：

* $r(A,F)$ 和 $r(B,F)$：分别表示源图像 $A$、$B$ 与融合图像 $F$ 之间的相关系数。
* $\bar{X}$ 和 $\bar{F}$：分别是图像 $X$ 和融合图像 $F$ 的平均像素值。
* $X(i,j)$ 和 $F(i,j)$：位置 $(i,j)$ 的像素值。
* $\sum$：求和符号，表示对所有像素位置的贡献进行累加。
* 根号内的乘积：用于归一化相关性计算，保证结果在 -1 到 1 之间。


***


#### **Code**：

```python
    @classmethod
    def CC(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        rAF = np.sum((image_A - np.mean(image_A)) * (image_F - np.mean(image_F))) / np.sqrt(
            (np.sum((image_A - np.mean(image_A)) ** 2)) * (np.sum((image_F - np.mean(image_F)) ** 2)))
        rBF = np.sum((image_B - np.mean(image_B)) * (image_F - np.mean(image_F))) / np.sqrt(
            (np.sum((image_B - np.mean(image_B)) ** 2)) * (np.sum((image_F - np.mean(image_F)) ** 2)))
        return (rAF + rBF) / 2
```



### (2) Sum of the Correlations of Differences <span style="color: #589bd5;">(SCD)</span>

* **Chinese**：**差异相关和**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越高越好</span>
* **Description**：差异相关和 (SCD) 用于衡量融合图像与源图像之间的差异，具体通过计算源图像与融合图像的差异图像之间的相关性来进行评估。SCD 值越高，表示融合图像保留了更多源图像的信息，质量越好。

* * *

#### **Formulas**：

$$
SCD = r(A,D_{A,F}) + r(B,D_{B,F})
$$

**element**：

* $r(A, D_{A,F})$ 和 $r(B, D_{B,F})$：分别表示源图像 $A$、$B$ 与它们与融合图像 $F$ 的差异图像 $D_{A,F}$、$D_{B,F}$ 之间的相关系数。
* $D_{A,F}$：表示源图像 $A$ 和融合图像 $F$ 之间的差异图像，即 $D_{A,F} = F - A$。
* $D_{B,F}$：表示源图像 $B$ 和融合图像 $F$ 之间的差异图像，即 $D_{B,F} = F - B$。

#### **Code**：

```python
    @classmethod
    def SCD(cls, image_F, image_A, image_B): # The sum of the correlations of differences
        cls.input_check(image_F, image_A, image_B)
        # 计算差异图像
        imgF_A = image_F - image_A
        imgF_B = image_F - image_B
        corr1 = np.sum((image_A - np.mean(image_A)) * (imgF_B - np.mean(imgF_B))) / np.sqrt(
            (np.sum((image_A - np.mean(image_A)) ** 2)) * (np.sum((imgF_B - np.mean(imgF_B)) ** 2)))
        corr2 = np.sum((image_B - np.mean(image_B)) * (imgF_A - np.mean(imgF_A))) / np.sqrt(
            (np.sum((image_B - np.mean(image_B)) ** 2)) * (np.sum((imgF_A - np.mean(imgF_A)) ** 2)))
        return corr1 + corr2
```

***


### (3) Nonlinear Correlation Coefficient <span style="color: #589bd5;">(NCC)</span>

* **Chinese**：**非线性相关系数**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越高越好</span>
* **Description**：非线性相关系数（NCC）用于衡量融合图像与源图像之间的非线性关系。该指标用于评估融合图像能否有效地保持源图像中非线性特征的相关性。NCC 越高，表示融合图像与源图像的非线性相关程度越高，融合效果越好。

* * *

#### **Formulas**：

$$
NCC = \frac{NCC_{AF} + NCC_{bF}}{2}
$$

$$
NCC_{XF} = 2 + \sum_{i} S_i \log \frac{S_i}{S_b} S'_i
$$

**element**：

* $S_i$：第 $i$ 个秩中样本分布的数量。
* $S_b$：秩的总数。
* $S$：样本对的总数。
* $S'_i$：样本的变动量。

#### **Code**：

```python
    @classmethod
    def calculate_NCC(cls, image_F, image_A, image_B):
        '''
        非线性相关系数 (NCC)
        '''
        cls.input_check(image_F, image_A, image_B)

        def calc_NCC(img1, img2):
            # 标准化图像
            img1_mean_sub = img1 - np.mean(img1)
            img2_mean_sub = img2 - np.mean(img2)
            norm1 = np.linalg.norm(img1_mean_sub)
            norm2 = np.linalg.norm(img2_mean_sub)
            ncc = np.sum(img1_mean_sub * img2_mean_sub) / (norm1 * norm2)
            return ncc

        # 计算融合图像与两个源图像的NCC并取平均
        NCC_FA = calc_NCC(image_F, image_A)
        NCC_FB = calc_NCC(image_F, image_B)
        return (NCC_FA + NCC_FB) / 2
```



---



## 5.Human Perception-Based

### (1)  The visual information fidelity for fusion <span style="color: #589bd5;">(VIFF)</span>

* **Chinese**：**视觉信息保真度**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越接近1越好</span>
* **Description**：视觉信息保真度（VIFF）是一种基于视觉信息保真度的图像质量评估指标，广泛应用于图像融合质量的评估。VIFF 通过衡量图像在不同频带下的条件信息量来评价图像的融合效果。值越大表示融合图像的质量越高，能够保留更多的源图像信息。

* * *

#### **Formulas**：

$$
VIF_{X,F} = \frac{\sum_{i \in \text{subbands}} I(C^{S,i}, F^{S,i} \mid R^{S,i})}{\sum_{i \in \text{subbands}} I(C^{S,i}, X^{S,i} \mid R^{S,i})}
$$

**elemen**：

* $I(C^{S,i}, F^{S,i} \mid R^S_i)$ 和 $I(C^{S,i}, X^{S,i} \mid R^S_i)$：分别表示条件信息量，表示在给定参考信号 $R^S_i$ 的情况下，通道 $C$ 中融合图像 $F$ 和源图像 $X$ 的信息量。
* $\text{subbands}$：图像被分割的频带，表示不同频率范围内的信息。
* $C^{S,i}$、$F^{S,i}$ 和 $X^{S,i}$：分别表示在第 i 个频带上的图像特征。

* * *

#### **Code**：

```python
    @classmethod
    def VIFF(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        return cls.compare_viff(image_A, image_F)+cls.compare_viff(image_B, image_F)

    @classmethod
    def compare_viff(cls,ref, dist): # viff of a pair of pictures
        sigma_nsq = 2
        eps = 1e-10

        num = 0.0
        den = 0.0
        for scale in range(1, 5):

            N = 2 ** (4 - scale + 1) + 1
            sd = N / 5.0

            # Create a Gaussian kernel as MATLAB's
            m, n = [(ss - 1.) / 2. for ss in (N, N)]
            y, x = np.ogrid[-m:m + 1, -n:n + 1]
            h = np.exp(-(x * x + y * y) / (2. * sd * sd))
            h[h < np.finfo(h.dtype).eps * h.max()] = 0
            sumh = h.sum()
            if sumh != 0:
                win = h / sumh

            if scale > 1:
                ref = convolve2d(ref, np.rot90(win, 2), mode='valid')
                dist = convolve2d(dist, np.rot90(win, 2), mode='valid')
                ref = ref[::2, ::2]
                dist = dist[::2, ::2]

            mu1 = convolve2d(ref, np.rot90(win, 2), mode='valid')
            mu2 = convolve2d(dist, np.rot90(win, 2), mode='valid')
            mu1_sq = mu1 * mu1
            mu2_sq = mu2 * mu2
            mu1_mu2 = mu1 * mu2
            sigma1_sq = convolve2d(ref * ref, np.rot90(win, 2), mode='valid') - mu1_sq
            sigma2_sq = convolve2d(dist * dist, np.rot90(win, 2), mode='valid') - mu2_sq
            sigma12 = convolve2d(ref * dist, np.rot90(win, 2), mode='valid') - mu1_mu2

            sigma1_sq[sigma1_sq < 0] = 0
            sigma2_sq[sigma2_sq < 0] = 0

            g = sigma12 / (sigma1_sq + eps)
            sv_sq = sigma2_sq - g * sigma12

            g[sigma1_sq < eps] = 0
            sv_sq[sigma1_sq < eps] = sigma2_sq[sigma1_sq < eps]
            sigma1_sq[sigma1_sq < eps] = 0

            g[sigma2_sq < eps] = 0
            sv_sq[sigma2_sq < eps] = 0

            sv_sq[g < 0] = sigma2_sq[g < 0]
            g[g < 0] = 0
            sv_sq[sv_sq <= eps] = eps

            num += np.sum(np.log10(1 + g * g * sigma1_sq / (sv_sq + sigma_nsq)))
            den += np.sum(np.log10(1 + sigma1_sq / sigma_nsq))

        vifp = num / den

        if np.isnan(vifp):
            return 1.0
        else:
            return vifp
```


***


### (2) Human Visual Perception <span style="color: #589bd5;">($Q_{CB}$)</span>

* **Chinese**：**人类视觉感知**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越接近1越好</span>
* **Description**：**$Q_{CB}$** 是一个基于人类视觉系统的指标，用于衡量融合图像与源图像之间主要特征的相似度。该指标从人类视觉感知的角度出发，越大的Q_CB值表示融合图像能更好地保留源图像中的关键信息。Q_CB值越大，说明融合图像越接近源图像的视觉感知效果，融合效果越好。

* * *

#### **Formulas**：

$$
Q_{CB} = \frac{1}{MN} \left( \sum_{i=1}^{N} \sum_{j=1}^{N} \beta_{A}(i,j) W_{A,F}(i,j) + \beta_B(i,j) W_{B,F}(i,j) \right)
$$

**element**：

* $Q_{CB}$：表示人类视觉感知的质量评估值。
* $\beta_{A}(i,j)$ 和 $\beta_B(i,j)$：显著图，表示源图像 $A$ 和 $B$ 在位置 $(i,j)$ 处的显著性。显著图反映了图像中哪些部分在视觉上更为突出。
* $W_{A,F}(i,j)$ 和 $W_{B,F}(i,j)$：表示从源图像 $A$ 和 $B$ 中转移到融合图像 $F$ 的对比度。即衡量源图像中每个像素的对比度对融合图像的贡献。

* * *

#### **Code**：

```python
    @classmethod
    def Q_CB(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        # 计算显著性图（简化版）
        beta_A = cls.calculate_significance_map(image_A)
        beta_B = cls.calculate_significance_map(image_B)
    
        # 计算对比度加权值
        W_AF = cls.calculate_contrast_weight(image_F, image_A)
        W_BF = cls.calculate_contrast_weight(image_F, image_B)
    
        # 计算Q_CB值
        Q_CB = np.sum(beta_A * W_AF + beta_B * W_BF) / (image_F.shape[0] * image_F.shape[1])
        return Q_CB
    
    @classmethod
    def calculate_significance_map(cls, image):
        """
        计算图像的显著性图
        """
        # 这里假设使用一个简单的对比度测量来代表显著性图
        grad_x = np.gradient(image, axis=1)
        grad_y = np.gradient(image, axis=0)
        grad_mag = np.sqrt(grad_x**2 + grad_y**2)
        significance_map = grad_mag / np.max(grad_mag)  # 标准化显著性图
        return significance_map
    
    @classmethod
    def calculate_contrast_weight(cls, image_F, image_X):
        """
        计算对比度加权值
        """
        grad_F_x = np.gradient(image_F, axis=1)
        grad_F_y = np.gradient(image_F, axis=0)
        grad_X_x = np.gradient(image_X, axis=1)
        grad_X_y = np.gradient(image_X, axis=0)
        
        grad_F_mag = np.sqrt(grad_F_x**2 + grad_F_y**2)
        grad_X_mag = np.sqrt(grad_X_x**2 + grad_X_y**2)
        
        contrast_weight = grad_F_mag / (grad_X_mag + 1e-10)  # 防止除零错误
        return contrast_weight
```

* * *

## 6.Error-Based

### (1) Mean Square Error <span style="color: #589bd5;">(MSE)</span>

* **Chinese**：**均方误差**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越小越好</span>
* **Description**：MSE 是一种基于像素误差的图像质量评估指标，它用来衡量融合图像与参考图像之间的差异。MSE值越小，表示融合图像与参考图像越接近，图像质量越好。

* * *

#### **Formulas**：

$$
MSE = \frac{MSE_{A,F} + MSE_{B,F}}{2}\\
MSE_{X,F} = \frac{1}{MN} \sum_{i=0}^{M-1} \sum_{j=0}^{N-1} (X(i,j) - F(i,j))^2
$$

**element**：

* $MSE_{A,F}$ 和 $MSE_{B,F}$：分别表示源图像 $A$、$B$ 与融合图像 $F$ 之间的均方误差。
* $X(i,j)$ 和 $F(i,j)$：分别是图像 $X$ 和融合图像 $F$ 在位置 $(i,j)$ 的像素值。
* $MN$：图像的总像素数，其中 $M$ 是行数，$N$ 是列数。
* $\sum$：求和符号，表示对所有像素位置进行累加。
* 归一化均方根误差（normalized root mean square error）就是将RMSE的值变成(0,1)之间。

* * *

#### **Code**：

```python
    @classmethod
    def MSE(cls, image_F, image_A, image_B):  # MSE
        cls.input_check(image_F, image_A, image_B)
        return (np.mean((image_A - image_F) ** 2) 
                + np.mean((image_B - image_F) ** 2)) / 2
```

***

### (2) Peak signal to noise ration <span style="color: #589bd5;">(PSNR)</span>

* **Chinese**：**峰值信噪比**
* **Calculation Object**: **Multiple Images**
* **Evaluation Direction**：<span style="color: #d6292a; font-weight: bold;">越大越好</span>
* **Description**：PSNR 是图像质量评估中的一个常用指标，主要用于衡量图像重建的质量。它基于图像中的信号与噪声的比率，PSNR值越大，表示融合图像的质量越好，通常用于衡量图像之间的失真程度。PSNR的值越高，说明图像的失真越少，质量越好。

* * *

#### **Formulas**：

$$
\mathrm{PSNR}=10\log\frac{\mathrm{z}^2}{\mathrm{MSE}}
$$

$$
MSE = \frac{MSE_{A,F} + MSE_{B,F}}{2}
$$

其中，

$$
MSE_{X,F} = \frac{1}{MN}\sum_{i = 0}^{M-1} \sum_{j = 0}^{N-1} ( X( i ,j) - F( i ,j) )^{2}
$$

**element**：

* $\mathrm{z}$：图像可能的最大像素值（例如，对于8位图像是255）。
* $\mathrm{MSE}$：均方误差，表示原始图像与重建图像之间像素值差异的平方的平均值。
* $\log_{10}$：以10为底的对数，用于将比率转换为分贝值（dB）。

* * *

#### **Code**：

```python
    @classmethod
    def PSNR(cls, image_F, image_A, image_B):
        cls.input_check(image_F, image_A, image_B)
        return 10 * np.log10(np.max(image_F) ** 2 / cls.MSE(image_F, image_A, image_B))
        //直接用img_F的z的平方除以之前就算好的MSE。
```


***


## Reference

[图像融合评估指标Python版：CSDN](https://blog.csdn.net/fovever_/article/details/129332278)

[图像融合通用评估指标：CSDN](https://blog.csdn.net/m0_49016094/article/details/136712567)

[CDDFuse:CVPR2023](https://github.com/Zhaozixiang1228/MMIF-CDDFuse/)
