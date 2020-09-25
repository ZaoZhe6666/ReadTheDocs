### 评测算法总览

平台已集成的评测算法及简要说明如下所示(使用评测算法缩写字典序排序)

#### 第一类评测算法

第一类评测算法指：针对模型行为鲁棒性，通过对输入样本添加对抗噪音，测试在对抗噪音攻击下模型鲁棒性的一类评测算法。该类评测算法存储的主要路径为：

```
~/AISafety/EvalBox/Evaluation
```

| 评测算法名称 | 评测方法缩写 | 评测方法简要说明 |
| --- | --- | ------- |
| clean accuracy | ACC | 未受攻击时，模型预测准确率 |
|      |   ACAC   |    对错误类别的平均预测置信度。其定义为经过对抗攻击后，对于所有攻击成功对抗样本（FGSM、PGD、C&W），所有误分类类别的平均概率。   |
|      |   ACTC   |   正确类别平均置信度。通过对对抗攻击样本（FGSM、PGD、C&W）的真实类，计算预测可信度的平均值。用来评估攻击在多大程度上偏离真实值。    |
|      |   ALDp   |    平均Lp失真度（使用I-FGSM，对于每个样本，直到模型产生误分类）。具体来说，计算扰动后发生改变的像素数量；计算原始示例和扰动示例之间的欧氏距离；测量对抗样本全维度下最大变化量。ALDp为所有攻击成功的对抗样本的平均归一化Lp失真度，ALDp越小，对抗样本的不可感知性越强。   |
|  |  ASS  |   平均结构相似性（使用I-FGSM，对于每个样本，直到模型产生误分类）。为了评估对抗样本的不可感知性，ASS被定义为所有攻击成功对抗样本与其原始样本间的平均相似性。  |
|  | PSD | 扰动敏感距离。用于评测人类对扰动的感知能力。其中m为像素点总数，表示第i个样例的第j个像素点，表示附近平方区域，std表示标准偏差函数。  |
|      |   NTE   |    NTE计算了误分类概率与其他类最大概率之间的差值。，其中，且    |
|      |   RGB   |    高斯模糊前后分类准确率比值   |
|      |    RIC  |    图像压缩前后分类准确率比值   |

#### 第二类评测算法

第二类评测算法指：针对模型行为鲁棒性，通过对输入样本添加自然噪音，测试在自然噪音攻击下模型鲁棒性的一类评测算法。该类评测算法存储的主要路径为：

```
~/AISafety/EvalBox/UserEvaluation
```

| 评测算法名称 | 评测方法缩写 | 评测方法简要说明 |
| --- | --- | ------ |
| Mean Corruption Error |  MCE |  计算神经网络对自然噪音的平均error值 |
| relative Mean Corruption Error |  RMCE | 计算神经网络在自然噪音下，error与vanilla差值 |
|  Mean Flip Probability  | MFP  | 计算神经网络对自然噪音序列的error  |
|   Mean Top-5 Distance  | MT5D  | 神经网络Top5的预测不一致性  |

#### 第三类评测算法

第三类评测算法指：为了提高模型鲁棒性，通过对抗训练或其他加固方式，得到加固后模型，针对模型加固前后鲁棒性变化及预测能力变化进行的评测方法。该类评测算法存储的主要路径为：

```
~/AISafety/EvalBox/Evaluation
```

| 评测算法名称 | 评测方法缩写 | 评测方法简要说明 |
| --- | --- | ------ |
|  |  CAV |  分类准确度方差，模型防御前后准确度的差 |
|  |  CRR | 模型防御后，减少的错误分类百分比 |
|    | CSR  | 模型防御后，增加的错误分类百分比  |
|     | CCV  | 测量防御造成的置信方差（对防御前后均分类正确样本做统计）  |
|   |  COS | 使用JS散度衡量模型防御前后输出概率相似性  |

#### 第四类评测算法

第四类评测算法指：针对模型静态结构，使用神经元覆盖等形式对模型进行评测的方法。该类评测算法存储的主要路径为：

```
~/AISafety/EvalBox/UserEvaluation
```

| 评测算法名称 | 评测方法缩写 | 评测方法简要说明 |
| --- | --- | ------ |
| Neuron Sensitivity |  SNS |  计算神经网络中神经单元对噪音敏感性 |
| epsilon-Empirical Noise Sensitivity |  ENI | 使用Lipschitz连续常数衡量模型对噪音的敏感性 |
|   Worst Case Boundary Distance | BD | 随机寻找N个正交的方向，然后计算对于一个模型来说每个方向需要移动多少可以改变instance的prediction label，来衡量模型的决策边界最小距离  |
|  Worst Case Boundary Distance-2   | BD2 | 上一种方法的变种，对于每一个direction（class），对于每一个instance使用I-FGSM来迭代移动n steps，计算n的大小来衡量模型的决策边界最小距离  |
|  Critical attacking route |  CAR | 描述神经网络的脆弱路径  |


#### 第五类评测算法

第五类评测算法指：针对进行模型训练或测试的数据集，根据模型在不同数据集上活跃表现，对模型预测行为加以可解释分析的方法。该类评测算法存储的主要路径为：

```
~/AISafety/EvalBox/UserEvaluation
```

| 评测算法名称 | 评测方法缩写 | 评测方法简要说明 |
| --- | --- | ------ |
| Neuron Coverage |  NC |  神经元覆盖率，用于衡量测试数据是否可以对神经网络神经元进行覆盖 |
| K-Multisection Neuron Coverage |  KMNC | K多节神经元覆盖 |
| Neuron Boundary Coverage | NBC | 神经元边界覆盖  |
|  Strong Neuron Activation Coverage | SNAC | 强神经元激活覆盖  |
|  Top-K Neuron Coverage |  TKNC | Top-k神经元覆盖 |
| Top-k Neuron Patterns  | TKNP | Top-k神经元模式 |