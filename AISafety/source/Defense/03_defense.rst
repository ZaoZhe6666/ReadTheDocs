加固防御模型调用说明
====================

平台已给出加固模型功能的集成调用文件，集成调用文件路径如下：

::

   ~/AISafety/test/test_defense.py 

下面将就Cifar-10数据集，给出一个完整的调用接口说明

加固防御模型调用参数说明
------------------------

test_defense.py文件中，主要逻辑代码如下：

::

   if __name__ == '__main__':
       parser = argparse.ArgumentParser(description='The Defense Model Generation')
       # common arguments
       # 用户所选用的加固算法缩写
       parser.add_argument(
           '--defense_method',
           type=str,
           nargs='*',
           default = "RAND")
       # 用户使用的数据集路径
       parser.add_argument(
           '--Data_path',
           type=str,
           nargs='*',
           default=["../Datasets/CIFAR_cln_data/cifar10_300_origin_inputs.npy",
                    "../Datasets/CIFAR_cln_data/cifar10_300_origin_labels.npy",
                    "../Datasets/CIFAR_cln_data/cifar10_300_origin_inputs.npy",
                    "../Datasets/CIFAR_cln_data/cifar10_300_origin_labels.npy" ])
       # 用户使用的数据集对应的字典文件
       parser.add_argument(
           '--Dict_path',
           type=str,
           default="./dict_lists/cifar10_dict.txt")
       # 用户使用的模型文件
       parser.add_argument(
           '--model',
           type=str,
           default ='Models.UserModel.ResNet2')
       # 执行攻击过程的batch_size大小
       parser.add_argument(
           '--batch_size', type=int, default=64, help='batch size')
       # arguments for the particular attack
       parser.add_argument(
           '--Scale_ImageSize',
           type=int,
           default=(32,32))
       parser.add_argument(
           '--Crop_ImageSize',
           type=int,
           default=(32,32))
       # 防御后结果文件存储路径
       parser.add_argument(
           '--Enhanced_model_save_path',
           type=str,
           default="./defense/")
       parser.add_argument(
           '--config_defense_param_xml_dir',
           type=str,
           default="./defense/EAT/EAT.xml")
       parser.add_argument(
           '--optim_config_dir',
           type=str,
           default="./defense/EAT/EAT_optim.xml")
       parser.add_argument(
           '--config_model_dir_path',
           type=str,
           default="./defense/EAT/EAT_model.xml")
       parser.add_argument(
           '--data_type',
           type=str,
           default='CIFAR10')
       # GPU设置
       parser.add_argument(
           '--GPU_Config',
           type=str,
           # 数目，index设置
           default=["2","0,1"])
       arguments = parser.parse_args()
       main(args=arguments)

其中涉及到的主要参数及说明如下：

defense_method
~~~~~~~~~~~~~~

该参数为防御算法的名称。目前已集成的防御算法为EAT，NAT，OAT，PAT以及RAND。详细说明请见防御算法具体说明一章。

Data_path
~~~~~~~~~

共包含四个参数位，分别表示：

1. 样本的数据集文件
   （白盒攻击下是原始样本，黑盒攻击下是攻击后的攻击样本）
2. 对应的标签的文件
   （非目标攻击下是groundtruth，目标攻击下是攻击目标类别）
3. 原始样本的数据集文件
4. 对应的groundtruth标签文件

Dict_path
~~~~~~~~~

对应的数据集的分类类别号码和类别名称的编号
default=“./dict_lists/cifar10_dict.txt”

model
~~~~~

用户打算使用的加固防御网络模型

batch_size
~~~~~~~~~~

default =64，读入数据每次批量处理的数目

Enhanced_model_save_path
~~~~~~~~~~~~~~~~~~~~~~~~

设置用来保存防御模型的路径，同时，设置相应的配置参数文件的位置

config_defense_param_xml_dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

用来设置防御方法的内部参数的配置文件，以EAT加固算法配套的EAT.xml文件为例：

.. code:: xml

   <method type='EAT'>
       <param title='dataset'>
           <dataset>CIFAR10</dataset>
       </param>
       
       <param title='epsilon'>
           <epsilon>0.6</epsilon>
       </param>
       
       <param title='train_externals'>
           <train_externals>False</train_externals>
       </param>
   </method>
       

config_model_dir_path
~~~~~~~~~~~~~~~~~~~~~

用来设置加固防御用的模型的内部参数的配置文件，以EAT加固算法配套的EAT_model.xml文件为例：

.. code:: xml

   <method type='ANP_VGG16'>
       <param title='batch_size'>
           <batch_size>64</batch_size>
       </param>
       
       <param title='enable_lat'>
           <enable_lat>False</enable_lat>
       </param>
   </method>

optim_config_dir
~~~~~~~~~~~~~~~~

用来设置训练加固防御用的模型的优化器的配置文件，以EAT加固算法配套的EAT_optim.xml文件为例：

.. code:: xml

   <method type='EAT'>
       <param title='optimizer'>
           <optimizer>optim. Adam(model.parameters(), lr=1e-2)</optimizer>
       </param>
       
       <param title='scheduler'>
           <scheduler>optim. lr_scheduler.StepLR(optimizer, 40, gamma=0.1)</scheduler>
       </param>
   </method>

这里因为优化器的函数比较多，用户需要写成字符串的表达式方式，进入计算的时候会用eval()函数调用该方法和设置的值

data_type
~~~~~~~~~

数据集类型，目前平台共支持以下三种：

1. CIFAR-10：default=“cifar10”
2. ImageNet：[‘ImageNet’, “withoutNormalize”]
3. ImageCustom

数据集的类型，目前是三种，根据实际注明类型，以方便预处理过程，另外使用ImageNet类型相似的数据一般不是原始的ImageNet数据集，选用[‘ImageNet’,
“withoutNormalize”]时候，不对图像做归一化，用户自行预处理

Scale_ImageSize
~~~~~~~~~~~~~~~

default=（32,32）（224,224）

cifar10，cifar100
默认到32,ImageNet推荐归一化到224X224。剩下的用户可以自定义，放缩到多少后再裁减

Crop_ImageSize
~~~~~~~~~~~~~~

default=（32,32）（224,224）

用户定义好的尺寸的缩放后，再裁减的后的尺寸

GPU_Config
~~~~~~~~~~

default=[“2”,“0,1”]

第一个表示，GPU的数目，第二个表示可以使用的GPU的编号。目前支持，在判断和实际设备中信息一致的设备中任意选取一个使用

加固防御模型调用运行说明
------------------------

以CIFAR-10数据集在ResNet2模型上进行加固为例：

相关文件路径
~~~~~~~~~~~~

::

   ├── EvalBox
   ├── Models
   │   ├── TestrModels
   │   ├── UserModels
   │   │   ├── utils
   │   │   ├── **ResNet2.py** // 使用的模型
   │   ├── weights
   │   ├── basic_module
   ├── utils
   ├── test
   │   ├── defense
   │   │   ├── EAT
   │   │   ├── **NAT** // 使用的防御算法
   │   │   ├── ...
   ├── Datasets

执行过程
~~~~~~~~

参照该文档前述的加固模型调用参数说明，修改对应参数内容，使用Python语言，执行对应加固训练过程。

::

   python test_defense.py

防御结果
~~~~~~~~

防御算法使用的是NAT，结果将会被保存在

::

   ~/AISafety/test/defense/NAT/CIFAR10_NAT_enhanced.pt

使用攻击算法FGSM执行测试，首先测试原始的resnet20_cifar.pt模型参数文件，得到攻击后准确率为0.1024
再对CIFAR10_NAT_enhanced.pt参数文件进行测试，得到攻击后准确率为0.2464
可以看出，加固后的网络对对抗样本的攻击防御有一部分上升
