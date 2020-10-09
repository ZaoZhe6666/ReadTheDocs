开始运行项目
============

运行项目前，需要先安装项目对应依赖环境

基础Python环境
--------------

::

   python==3.6.5

辅助环境
--------

::

       certifi==2020.6.20
       cloudpickle==1.6.0
       cycler==0.10.0
       dask==2.27.0
       decorator==4.4.2
       future==0.18.2
       imageio==2.9.0
       kiwisolver==1.2.0
       matplotlib==3.3.0
       networkx==2.5
       numpy==1.19.2
       opencv-python==3.4.2.16
       pandas==0.23.4
       Pillow==7.2.0
       pyparsing==2.4.7
       python-dateutil==2.8.1
       pytz==2020.1
       PyWavelets==1.1.1
       PyYAML==5.3.1
       scikit-image==0.17.2
       scipy==1.5.2
       six==1.15.0
       tifffile==2020.9.3
       toolz==0.10.0
       Wand==0.5.2

环境配置安装
------------

新建空环境
~~~~~~~~~~

|image1|

安装torch
~~~~~~~~~

torch的安装参照\ `官网 <https://pytorch.org/get-started/locally/>`__\ ，根据用户的环境选择合适的，例如在gpu下，要选择cuda的方式，cpu就按照cpu方式。

|image2|

|image3|

依赖安装
--------

安装的包参见 requirements.txt

::

   pip install -r requirement.txt

在GPU的方式下，需要根据硬件信息，安装对应的硬件驱动，cuda和cudnn版本

cpu下安装完成后的python安装包环境如下：

::

   Package                Version
   -------------------    -------------
   certifi                2020.6.20
   cloudpickle            1.6.0
   cycler                 0.10.0
   dask                   2.27.0
   decorator              4.4.2
   future                 0.18.2
   imageio                2.9.0
   kiwisolver             1.2.0
   matplotlib             3.3.0
   networkx               2.5
   numpy                  1.19.2
   opencv-python          3.4.2.16
   pandas                 0.23.4
   Pillow                 7.2.0
   pip                    10.0.1
   pyparsing              2.4.7
   python-dateutil        2.8.1
   pytz                   2020.1
   PyWavelets             1.1.1
   PyYAML                 5.3.1
   scikit-image           0.17.2
   scipy                  1.5.2
   setuptools             39.1.0
   six                    1.15.0
   tifffile               2020.9.3
   toolz                  0.10.0
   torch                  1.6.0+cpu
   torchvision            0.7.0+cpu
   Wand                   0.5.2

程序安装说明
------------

使用GitHub在开源地址下clone或者fork源码，并参考环境安装配置

.. |image1| image:: ../Pic/图片15.png
.. |image2| image:: ../Pic/图片16.png
.. |image3| image:: ../Pic/图片17.png
