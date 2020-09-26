扩展攻击算法
~~~~~~~~~~~~

攻击算法类图
------------------------------------------------------------------------

此处应该有个图片

如上图所示，上图为攻击算法FGSM和攻击算法SPSA继承Attack类示意图。攻击算法内部可自行实现攻击过程具体函数，仅需覆写generate函数即可。

攻击算法存储位置
------------------------------------------------------------------------

::

  ├── EvalBox
  │   ├── **Attack**
  │   │   ├──AdvAttack  对抗攻击算法存储路径
  │   │   │   ├──__init__.py
  │   │   │   ├──attack.py
  │   │   │   ├──fgsm.py
  │   │   │   ├──....py
  │   │   ├──CorAttack  噪声攻击算法存储路径
  │   ├── Analysis
  │   ├── Defense
  │   ├── Evaluation
  ├── Models
  ├── utils
  ├── test
  ├── Datasets

扩展实例——FGSM算法
------------------------------------------------------------------------

FGSM算法路径为：

::

  ~/AISafety/EvalBox/Attack/fgsm.py

FGSM算法源代码:

::

  import numpy as np
  import torch
  from torch.autograd import Variable
  from EvalBox.Attack.AdvAttack.attack import Attack
  from utils.CrossEntropyLoss import CrossEntropyLoss

  class FGSM(Attack):
      def __init__(self, model=None, device=None,IsTargeted=None, **kwargs):
          '''
          @description: Fast Gradient Sign Method (FGSM) 
          @param {
              model:需要测试的模型
              device: 设备(GPU)
              IsTargeted:是否是目标攻击
              kwargs: 用户对攻击方法需要的参数
          } 
          @return: None
          '''
          super(FGSM, self).__init__(model, device,IsTargeted)
          #使用该函数时候，要保证训练模型的标签是从0开始，而不是1
          self.criterion = torch.nn.CrossEntropyLoss()
          self._parse_params(**kwargs)

      def _parse_params(self, **kwargs):
          '''
          @description: 
          @param {
              epsilon:沿着梯度方向步长的参数
          } 
          @return: None
          '''
          self.eps = float(kwargs.get('epsilon', 0.03))

      def generate(self, xs=None, ys=None):
          '''
          @description: 
          @param {
              xs:原始的样本
              ys:样本的标签
          } 
          @return: adv_xs{numpy.ndarray}
          '''
          device = self.device
          self.model.eval().to(device)
          targeted=self.IsTargeted
          print("targeted", targeted)
          copy_xs = np.copy(xs.numpy())
          var_xs = torch.tensor(copy_xs, dtype=torch.float,     device=device, requires_grad=True)
          var_ys = torch.tensor(ys, device=device)
          outputs = self.model(var_xs)
          preds = torch.argmax(outputs, 1)
          preds = preds.data.cpu().numpy()
          if targeted:
             loss = -self.criterion(outputs, var_ys)
          else:
              loss = self.criterion(outputs, var_ys)
          loss.backward()
          grad_sign = var_xs.grad.data.sign().cpu().numpy()
          copy_xs = np.clip(copy_xs + self.eps * grad_sign, 0.0, 1.0)
          adv_xs = torch.from_numpy(copy_xs)
          return adv_xs

扩展说明
------------------------------------------------------------------------

1. 用户需要实现个人攻击算法，并继承基础的Attack类

2. 用户需要将待扩展的攻击算法对应文件，如new_attack_method.py，放置于以下路径中

::

  ~/AISafety/EvalBox/Attack/

3. 用户需要在2中路径下的__init__.py文件中，添加用户攻击算法类的引用：

::

  from .attack import Attack
  from .fgsm import FGSM
  ...
  from .new_attack_method import NEW_ATTACK_METHOD

4. 用户需要在test/attack_param路径下，生成个人预设参数txt文件，及参数xml文件。若仅需要默认参数，则xml文件可为空。

5. 用户可在集成调用文件testimport.py中，修改attack_method参数为NEW_ATTACK_METHOD