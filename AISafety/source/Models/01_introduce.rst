测试模型
========

已集成内容
----------

目前项目已集成的测试模型共有以下几个：

-  针对Cifar-10数据集的ResNet20，FP_ResNet，VGG16
-  针对ImageNet数据集的VGG19

目前项目已集成的模型参数共有以下几个：

-  resnet20模型在cifar10原始训练集上模型参数
-  resnet20模型通过RAND加固算法加固后模型参数
-  FP_ResNet模型在cifar10原始训练集上模型参数
-  VGG16模型在cifar10原始训练集上模型参数
-  VGG16模型通过RAND加固算法加固后模型参数

自定义模型
----------

用户可按照模型扩展要求，实现并上传自定义模型。

也可默认l 使用pytorch自带的模型，以及默认的预训练模型，具体有例如：

-  torchvision.model.vgg19

-  torchvision.model.alexnet
