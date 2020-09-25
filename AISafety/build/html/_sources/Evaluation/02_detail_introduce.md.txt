### 评测算法详细介绍

平台已集成的评测算法及简要说明如下所示(使用评测算法缩写字典序排序)

#### ACC 精确度

模型的分类精确度（Accuracy，ACC）。其定义为经过对抗攻击后，模型对于所有攻击样本中依然能够分类正确的样本占总体样本的比值。公式定义如下：


$$
ACC = \frac{n}{N}
$$

其中，n代表所有对抗样本中分类正确的数量，N代表对抗样本的总数。



#### ACAC 平均置信度

对错误类别的平均预测置信度(Average Confidence of Adversarial Class)。其定义为经过对抗攻击后，对于所有攻击成功对抗样本，所有误分类类别的平均概率。公式定义如下

$$
ACAC=\frac{1}{n} \sum_{i = 1}^{n}P\left ( X_{i}^{a} \right )_{F\left ( X_{i}^{a} \right )}
$$

其中n表示所有对抗攻击成功样本个数，

$$
F\left ( X_{i}^{a} \right )
$$

表示第i个样本被分类为a类，$ P\left ( X\_{i}^{a} \right ) $ 为第i个样本被分类为a类的概率。



#### ACTC 对抗成功时正确标签平均置信度

正确类别平均置信度(Average Confidence of True Class)。通过对对抗攻击样本的真实类，计算预测可信度的平均值。用来评估攻击在多大程度上偏离真实值。

$$
ACTC=\frac{1}{n} \sum_{i = 1}^{n}P\left ( X_{i}^{a} \right )_{ y_{i}}
$$

其中$ P\left ( X\_{i}^{a} \right ) $ 为第i个样本被分类为对应类的概率，yi表示第i个样本被正确分类为yi类。



#### ALDp 对抗攻击失真度

平均Lp失真度(Average Lp Distortion)。几乎所有的攻击都采用Lp norm距离（p=0, 2, ∞）作为评价的失真度量。具体来说，L0计算扰动后发生改变的像素数量；L2计算原始示例和扰动示例之间的欧氏距离；L∞测量对抗样本全维度下最大变化量。ALDp为所有攻击成功的对抗样本的平均归一化Lp失真度，ALDp越小，对抗样本的不可感知性越强。

$$
ALDp=\frac{1}{n}\sum_{i=1}^{n}\frac{\left \| X_{i}^{a} - X_{i}  \right \|_{p}} { \left \| X_{i} \right \|_{p}}
$$

其中n为所有攻击成功的对抗样本个数。



#### ASS 对抗样本数据与原数据的结构相似度（SSIM）

平均结构相似性(Average Structural Similarity)。SSIM作为量化两幅图像间相似性常用指标之一，被认为比Lp相似度更符合人类视觉感知。为了评估对抗样本的不可感知性，ASS被定义为所有攻击成功对抗样本与其原始样本间的平均相似性，即

$$
ASS=\frac{1}{n}\sum_{i=1}^{n}SSIM\left ( X_{i}^{a}, X_{i} \right )
$$

其中n表示所有攻击成功的对抗样本个数。ASS值越大，则对抗样本的不可感知性越强。



#### BD 最大边界距离

最大边界距离（Worst Case Boundary Distance）。数据点之间到决策边界的距离衡量模型在最坏情况下的稳定性和鲁棒性。

随机寻找N个正交的方向，然后计算对于一个模型来说每个方向需要移动多少可以改变instance的prediction label，来衡量模型的决策边界最大距离。
$$
BD = \frac{1} {N} \sum_{i = 1}^{N} d_{i}, d_{i}=max \phi _{i} \left ( V \right )
$$
其中，V表示一个随机生成的集合，$ \phi_{i} (V) $为到模型决策边界的RMS距离，di为到决策边界距离的最大值。



#### CAV 分类准确度方差

分类准确度方差(Classiﬁcation Accuracy Variance)。用于评估深度学习模型性能的最重要指标为准确性，因此，防御增强模型应尽可能保持常规测试实例下的分类精度。通过CAV评估防御对模型分类精度的影响，定义为
$$
CAV = Acc(F^{D},T)-Acc(F,T)
$$
其中Acc(F,T)表示模型F在数据集T下的识别准确率。



#### CCV 分类置信方差

分类置信方差(Classification Confidence Variance)。对原始模型增加防御，可能不会影响评估准确率，但是正确分类样本的预测可信度可能会降低，因此引入CCV，测量防御增强模型引起的置信方差，我们定义
$$
CCV=\frac{1}{n}\sum_{i = 1}^n|P(X_i)_{y_i}-P^D(X_i)_{y_i}|
$$
其中n表示在原始模型及防御增强后模型全部分类准确的样本数量。



#### COS 分类输出稳定性

COS全称为分类输出稳定性(Classification Output Stability)。使用JS散度来衡量原始模型和防御增强模型输出概率相似性，即分类输出的稳定性。

对所有正确分类的测试实例，定义
$$
COS = \frac{1}{n}\sum_{i=1}^nJSD(P(X_i)||P^D(X_i))
$$
其中n表示在原始模型及防御增强后模型全部分类准确的样本数量，JSD表示计算JS散度的函数。COS值越低，两个模型的差距越小



#### CRR/CSR 分类校正/补偿比率

分类校正/补偿比率(Classification Rectify/Sacrifice Ratio)。为了评估防御对模型在测试集上预测结果的影响，将CRR定义为原始模型错误分类，但增加防御后，模型正确分类的测试实例的百分比。与此相反，CSR表示原始模型正确分类，但增加防御后，模型错误分类的测试实例的百分比。
$$
\\ CRR=\frac{1}{N}\sum_{i=1}^Ncount(F(X_i) \neq y_i \& F^D(X_i)=y_i)
\\ CSR=\frac{1}{N}\sum_{i=1}^Ncount(F(X_i) = y_i \& F^D(X_i) \neq y_i)
$$
同时CAV = CRR - CSR。其中F表示原始模型预测结果，![img](file:///C:\Users\赵二狗\AppData\Local\Temp\ksohtml11112\wps2.jpg)表示防御后模型预测结果，&符号表示同时满足。返回值越小，两个模型的差距越小。



#### ENI

ε-Empirical Noise Insensitivity（综合对抗攻击和自然噪音的一个test set）。ENI值越低，攻击的不可感知性越强。



#### NTE 噪声容量估计

噪声容量估计(Noise Tolerance Estimation)，对抗样本的鲁棒性可通过噪声容限来估计，噪声容限反映了对抗样本在保持分类类别不变的情况下，可容忍的噪声量。具体来说，NTE计算了误分类概率与其他类最大概率之间的差值。
$$
NTE = \frac{1}{n}\sum_{i=1}^n [ P(X_i^a)_{F(X_i^a)} - max{P(X_i^a)_j}]
，其中j\in\{1,...,k\},且j \neq F(X_i^a)
$$
NTE值越高，说明对抗样本的鲁棒性越高，攻击算法的鲁棒性越强。



#### PSD 扰动敏感距离

扰动敏感距离(Perturbation Sensitivity Distance)。用于评测人类对扰动的感知能力。
$$
PSD=\frac{1}{n} \sum_{i=1}^n \sum_{j=1}^m \frac{\delta_{i,j}}{std(R(x_{i,j}))}
$$
其中m为像素点总数，$ \delta_{i, j} $ 表示第i个样例的第j个像素点，$ R(x_{i,j}) $ 表示 $ x_{i, j} $ 附近平方区域，std表示标准偏差函数。PSD的值越小，则对抗样本的不可感知性越强



#### RGB 对高斯模糊鲁棒性

对高斯模糊鲁棒性(Robustness to Gaussian Blur)。高斯模糊常被用于计算机视觉算法中的图像去噪。正常情况下，一个高鲁棒性的对抗样本，在高斯模糊后应保持其误分类效果。

可以定义
$$
\\RGB_{UA}=\frac{count(F(GB(X_i^a))\neq y_i)}{count(F(X_i^a)\neq y_i)}
\\RGB_{TA}=\frac{count(F(GB(X_i^a))= y_i^+)}{count(F(X_i^a)= y_i^+)}
$$
UA表示非定向攻击，TA表示定向攻击，GB函数表示高斯模糊处理。RGB结果越高，说明对抗样本鲁棒性越强。



#### RIC 对图像压缩鲁棒性

对图像压缩鲁棒性(Robustness to Image Compression)。图像压缩常被用于计算机视觉算法中的图像去噪。正常情况下，一个高鲁棒性的对抗样本，在图像压缩后应保持其误分类效果。

可以定义
$$
\\RIC_{UA}=\frac{count(F(IC(X_i^a))\neq y_i)}{count(F(X_i^a)\neq y_i)}
\\RIC_{TA}=\frac{count(F(IC(X_i^a))= y_i^+)}{count(F(X_i^a)= y_i^+)}
$$
UA表示非定向攻击，TA表示定向攻击，IC函数表示图像压缩处理。RIC结果越高，说明对抗样本鲁棒性越强。