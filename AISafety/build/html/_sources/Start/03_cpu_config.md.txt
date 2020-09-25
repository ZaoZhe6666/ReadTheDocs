### CPU/GPU模式和设置说明

&nbsp;&nbsp;&nbsp;&nbsp;该平台会自动检测运行的环境是否存在GPU，默认需要安装cuda的环境中才可以使用GPU。

#### 使用CPU运行程序

&nbsp;&nbsp;&nbsp;&nbsp;无需设置，可自动识别

#### 使用单一/多GPU运行程序

&nbsp;&nbsp;&nbsp;&nbsp;用户需要在集成调度接口`~/test/testimport.py`中，对参数`--GPU_Config`进行设置

```
parser.add_argument(
    '--GPU_Config',
    type=str,
    # 数目，index设置
    default=["2","0,1"])
```

**说明：**

&nbsp;&nbsp;&nbsp;&nbsp;默认输入的是GPU数目，第二个是GPU的编号，程序会根据用户输入的信息自动校验实际运行环境的GPU情况，合理做出判断，并指定一个可用的GPU设备号作为运行用的设备