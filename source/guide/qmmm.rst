.. _qmmm:

QM/MM建模
================================================

想要实现QM/MM输入参数生成可在全原子力场模块中依次实现创建分子结构——根据力场选择原子类型——生成拓扑文件，在最后 **选择计算软件** 选择量化Gaussian，ORCA和Amber软件即可，Amber对应BDF软件格式。

.. figure:: image/qmmm输入参数生成.png
    :align: center