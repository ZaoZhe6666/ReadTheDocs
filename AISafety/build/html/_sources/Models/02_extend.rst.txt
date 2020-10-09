扩展模型
========

模型存储位置
------------

::

   ├── EvalBox
   ├── Models
   │   ├── TestModels    测试用模型存储路径
   │   ├── UserModels    用户个人模型存储路径
   │   │   ├── utils
   │   │   ├── ResNet2.py
   │   │   ├── new_model.py  用户新上传模型
   │   ├── weights
   │   │   ├── resnet20_cifar.pt
   │   │   ├── new_weights.pt  用户新上传模型对应参数文件
   │   ├── basic_module
   ├── utils
   ├── test
   ├── Datasets

模型扩展要求
------------

如下为一个可使用模型实例：

.. code:: python

   import ...

   from Models.basic_module import BasicModule

   # 用户可自定义若干辅助方法
   def adjust_learning_rate(epoch, optimizer): 
       ...

   def conv3x3(in_planes, out_planes, stride=1): 
       ...

   # 用户可通过类，定义若干个人模型
   class BasicBlock(BasicModule): 
       ...

   class ResNet_Cifar(BasicModule):
       ...

   # 用户自定义实现的获取模型的方法
   def resnet20_cifar(thermometer = False, level = 1):
       model = ResNet_Cifar(BasicBlock, [3, 3, 3], thermometer = thermometer, level = level)
       # 返回值应为用户实现的模型文件中模型
       return model

   # 必须实现的公用方法getModel
   def getModel():
       return resnet20_cifar()

模型的格式仅需要遵循，实现getModel函数，返回模型。
