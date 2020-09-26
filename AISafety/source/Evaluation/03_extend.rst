扩展评测算法
~~~~~~~~~~~~

评测算法类图
------------------------------------------------------------------------

此处应该有个图片

如上图所示，上图为评测算法ACTC和评测算法CAV和评测算法CACC继承Evaluation类示意图。评测算法内部可自行实现评测过程具体函数，仅需覆写evaluate函数即可。

评测算法存储位置
------------------------------------------------------------------------

::

  ├── EvalBox
  │   ├── Attack
  │   ├── Analysis
  │   ├── **Evaluation**  评测算法存储位置
  │   │   ├──__init__.py
  │   │   ├──actc.py
  │   │   ├──CAV.py
  │   │   ├──evaluation.py
  │   │   ├──evaluation_defense.py
  │   ├── Defense
  ├── Models
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

1. 用户需要实现个人评测算法，并继承基础的Evaluation类，若用户希望实现的是比较模式的评测算法，则继承Evaluation_Defense类

2. 用户需要将待扩展的评测算法对应文件，如new_evaluate_method.py，放置于以下路径中

::

  ~/AISafety/EvalBox/Evaluation/

3. 用户需要在2中路径下的__init__.py文件中，添加用户评测算法类的引用：

::

  from .CAV import CAV
  from .acc import ACC
  ...
  from .new_evaluate_method import NEW_EVALUATE_METHOD

4. 用户可在集成调用文件testimport.py中，修改evaluation_method参数为NEW_EVALUATE_METHOD

5. 如果用户评测算法是针对动态行为的评测算法，即需要获取传入模型，可设置IS_PYTHORCH_WHITE参数为True

6. 若用户评测算法为对比模型，需设置IS_COMPARE_MODEL参数为True