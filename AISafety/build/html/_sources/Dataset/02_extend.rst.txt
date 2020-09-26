扩展数据集
================================================================================

平台代码分布结构
------------------------------------------------------------------------

::

    ├── EvalBox
    ├── Models
    ├── utils
    ├── **test**
    │   ├── testimport.py
    │   ├── testimport_black.py
    │   ├── **cifartonpy.py**
    │   ├── **dict_lists**
    │   │   ├── cifar10_dict.txt
    │   │   ├── ImageNet_12_dict.txt
    ├── **Datasets**
    │   ├── CIFAR_cln_data
    │   │   ├──cifar10_30_origin_inputs.npy
    │   │   ├──cifar10_30_origin_labels.npy
    │   │   ├──cifar10_30_target_labels.npy
    │   │   ├── ...
    │   ├──ImageNet
    │   │   ├──Images
    │   │   ├──val_20.txt
    │   ├──ImageCustom
    │   ├──adv_data
    │   │   ├──zjx.json

上图为与扩展用户个人数据集相关的平台结构图。目前平台共支持两类数据集：

1. npy格式数据集（与平台中cifar10，cifar100结构格式一致），包含原始样本，样本的标签。
2. 原始图像 + 图像标签txt文件的数据集。（与平台中ImageNet数据集格式一致）

需要注意的是，上述两类数据集，均需要额外提供对应的类别编号对应的目标名称的字典


扩展npy数据集
------------------------------------------------------------------------

平台目前提供了一个，将cifar数据集转换为npy数据格式的样例文件cifartonpy.py。

转换Cifar-10数据集
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


获得Cifar-10数据集的train数据Xtr，Ytr；test数据Xte，Yte：

::

	Xtr, Ytr, Xte, Yte=load_CIFAR10('../../cifar-10-python/cifar-10-batches-py')

保存Cifar-10的test数据的随机1500个,方式是随机均匀取10类,各150。这里的标签是原始的Groundtruth标签,IsTargeted=False

::

	save_numpy( Xte, Yte,'../Datasets/CIFAR_cln_data/',1500,shuff="random_equally",datasetType="cifar10",IsTargeted=False)

如果IsTargeted=True ,则是随机生成和原始样本的GroundTruth不一致的标签,可以用于目标攻击使用,用户也可以自行定义目标标签的生成类别规则。

其中'../../cifar-10-python/cifar-10-batches-py'是原始的cifar10数据下载下来的
返回值 Xtr, Ytr, Xte, Yte分别是 cifar10的train数据 Xtr,Ytr,cifar10的test数据。

::

	调用save_numpy(
        X     #全部的数据
      , Y     #全部的数据
      , path  #npy 数据保存的目标路径
      , number=10000   #最后的要保存的数据的数量，<=总的
      , shuff="random_equally" #随机选取的类别数量要均衡的方式
      , datasetType="cifar10"  #数据集的名称
      , IsTargeted=False   #是否生成的是用于目标攻击的标签，随机值（和原始的不同即可)
      )

转换Cifar-100数据集
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	numbertest=10000
	Xte100, Yte100=load_CIFAR100('../Datasets/CIFAR10/cifar-100-python','test',numbertest)
	save_numpy( Xte100, Yte100,'../Datasets/cln_data/',300,shuff="random_equally",datasetType="cifar100",IsTargeted=False)