Docker模式和设置说明
====================

Docker CE安装（Linux环境）
--------------------------

软件下载
~~~~~~~~

官方地址：\ `官网下载`_ 
阿里docker-ce镜像：\ `Docker镜像`_

内核要求
~~~~~~~~

docker 要求centos系统内核版本必须高于3.10
centos下通过 uname -r 查看内核版本。

软件安装
~~~~~~~~

1. **卸载旧版本docker相关包**

::

       sudo yum remove docker  docker-common docker-selinux docker-engine

2. **安装需要的软件包**

yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

::

           sudo yum install -y yum-utils device-mapper-persistent-data lvm2

.. _官网下载: https://www.docker.com/community-edition
.. _Docker镜像: https://developer.aliyun.com/mirror/docker-ce
   

3. **设置yum源**

设置官方源：

::

       sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

设置阿里源：

::

       sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

4. **刷新yum源**

::

       sudo yum makecache fast

5. **安装docker-ce**

查看全部版本：（如果匹配不到docker-ce包，执行4刷新yum源）

::

     yum list docker-ce --showduplicates | sort -r 

安装指定版本：

::

     sudo yum -y install docker-ce-18.03.0.ce

安装最新版：

::

     sudo yum -y install docker-ce

6. **启动&开机启动**

启动：

::

     systemctl start docker

开机启动：

::

     systemctl enable docker

重启（按顺序执行1,2）：

::

      systemctl daemon-reload
      systemctl restart docker 



7. **验证是否安装成功**

::

       docker version 

输出：

::

      Client:
      Version:       18.03.0-ce
      API version:   1.37
      Go version:    go1.9.4
      Git commit:    0520e24
      Built: Wed Mar 21 23:09:15 2018
      OS/Arch:       linux/amd64
      Experimental:  false
      Orchestrator:  swarm

     Server:
      Engine:
       Version:      18.03.0-ce
       API version:  1.37 (minimum version 1.12)
       Go version:   go1.9.4
       Git commit:   0520e24
       Built:        Wed Mar 21 23:13:03 2018
       OS/Arch:      linux/amd64
       Experimental: false

Docker使用
--------------------------

创建dockfile
~~~~~~~~~~~~~~~~

详见\ `dockfile创建说明`_\ ，或参考附件中样例，\ `下载链接`_

运行镜像
~~~~~~~~~~~~~~~~

将dockfile文件与项目放在同一路径下；进入项目路径，运行\ ``docker build``\ 创建镜像；

查看image镜像
~~~~~~~~~~~~~~~~

::

   docker images
   docker images -a
   docker image ls
   docker image ls -a

其他Docker命令
~~~~~~~~~~~~~~~~
详见\ `Docker官网`_

Docker运行
--------------------------

.. _dockfile创建说明: https://www.cnblogs.com/panwenbin-logs/p/8007348.html
.. _下载链接: https://www.cnblogs.com/panwenbin-logs/p/8007348.html
.. _Docker官网: https://docs.docker.com/engine/reference/commandline/cli/