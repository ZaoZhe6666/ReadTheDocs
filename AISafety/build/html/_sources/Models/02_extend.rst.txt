扩展模型
~~~~~~~~~~~~


模型存储位置
------------------------------------------------------------------------

::

  ├── EvalBox
  ├── Models
  │   ├── TestModels    测试用模型存储路径
  │   ├── UserModels    用户个人模型存储路径
  │   │   ├──utils
  │   │   ├──ResNet2.py
  │   ├── weights
  │   │   ├──resnet20_cifar.pt
  │   ├── basic_module
  ├── utils
  ├── test
  ├── Datasets

扩展实例——ACTC算法
------------------------------------------------------------------------

ACTC算法路径为：

::

  ~/AISafety/EvalBox/Evaluation/actc.py

ACTC算法源代码:

::

    class ACTC(Evaluation):
        def __init__(self, outputs_origin, outputs_adv, device, **kwargs):
            '''
            @description:
            @param {
                model:
                device:
                kwargs:
            }
            @return: None
            '''
            super(ACTC, self).__init__(outputs_origin, outputs_adv, device)

            self._parsing_parameters(**kwargs)
        def _parsing_parameters(self, **kwargs):
            '''
            @description:
            @param {
            }
            @return:
            '''

        def evaluate(self,adv_xs=None, cln_xs=None, cln_ys=None,adv_ys=None,target_preds=None, target_flag=False):
            '''
            @description:
            @param {
                adv_xs: 攻击样本
                cln_xs：原始样本
                cln_ys: 原始类别，非目标攻击下原始样本的类型
                adv_ys: 攻击样本的预测类别
                target_preds： 目标攻击下希望原始样本攻击的目标类别
                target_flag：是否是目标攻击
            }
            @return: actc {Average Confidence of True Class}
            '''
            total = len(adv_xs)
            print("total", total)
            outputs = torch.from_numpy(self.outputs_adv)
            number = 0
            prob = 0
            outputs_softmax=torch.nn.functional.softmax(outputs, dim=1)
            preds = torch.argmax(outputs, 1)
            outputs_softmax = outputs_softmax.data.numpy()
            preds = preds.data.numpy()
            labels = target_preds.numpy()
            if not target_flag:
                for i in range(preds.size):
                    if preds[i] != labels[i]:
                        number += 1
                        prob += outputs_softmax[i,labels[i]]
            else:
                for i in range(preds.size):
                    if preds[i] == labels[i]:
                        number += 1
                        prob += outputs_softmax[i, labels[i]]
            if not number == 0:
                actc = prob / number
            else:
                actc = prob/(number+MIN_COMPENSATION)
            return actc

扩展说明
------------------------------------------------------------------------

1. 用户需要实现个人模型，并添加getModel方法，返回用户个人模型

2. 用户需要将待扩展的模型文件，如user_model.py，放置于以下路径中

::

  ~/AISafety/Models/UserModel

3. 用户需要将待扩展的模型文件对应参数信息，如user_model.pth，放置于以下路径中

::

  ~/AISafety/Models/weights

4. 用户可在集成调用文件testimport.py中，修改model参数为"Models.UserModel.user_model",修改model_dir参数为"../Models/weights/user_model.pth"

5. 因为模型继承了BasicModule,注意传参有一个**kwargs,为了可以用户通过字典或xml文件方式配置模型需要的参数。例如使用xml方式配置优化器：

::

  <method type='RAND'>
  <param title='optimizer'>
    <optimizer>optim.Adam(model.parameters(), lr=1e-2)</optimizer>
  </param>
  <param title='scheduler'>
    <scheduler>optim.lr_scheduler.StepLR(optimizer, 40, gamma=0.1)</scheduler>
  </param>
  </method>