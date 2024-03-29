.. _Polymers:

聚合物建模
================================================

目前本模块支持均聚物、嵌段共聚物、无规共聚物建模、2D建模，此外还提供了较为完备的聚合物单体库，方便用户建立聚合物分子。

均聚物
-------------------------------------------------------
均聚物是一种单体聚合而成的聚合物。

.. figure:: image/聚合物建模-均聚物.png
    :align: center

.. note::
  勾选 **随机数种子** 选项，程序会自动随机地生成聚合物结构。当然对于单一单体而言，确定了两个连接点，实际上就没有随机性了。但是有多种组分的时候，随机性就会发生作用了。

嵌段共聚物
-------------------------------------------------------
嵌段共聚物是将两种或两种以上性质不同的聚合物链段连在一起制备而成的一种特殊聚合物。

.. figure:: image/聚合物建模-嵌段共聚物.png
    :align: center

无规共聚物
-------------------------------------------------------
现已支持两种方法构建无规聚合物链，分别是概率和竞聚率。当选择概率方法构建无规共聚物时，即给出每一个单体在下一步反应中连接到环上的可能性，并可以用于模拟浓度。对话框会出现n*n矩阵，该矩阵是按行来进行读入的。下图中，第一行表示，在聚合物链中，当上一个单体是对苯的时候，与之相连的下一个单体有50%的可能性是对苯，50%的可能性是噻二唑；第二行表示，当上一个单体是噻二唑的时候，与之相连的下一个单体有50%的可能性是对苯，50%的可能性是噻二唑。默认值是0.5，对于我们要求的组分比例为50:50来说刚好合适。 

.. figure:: image/聚合物建模-无规共聚物-概率.png
    :align: center

当选择竞聚率方法构建无规共聚物时，即用与实验合成相类似的方法测定共聚物成分，要求输入反应物浓度和多种连接选择的速率常数。竞聚率是反映共聚合反应特性的一个重要参数，表征单体的相对活性以及单体自聚和共聚倾向的大小。

.. figure:: image/聚合物建模-无规共聚物-竞聚率.png
    :align: center

.. note::

 当想得到一个具有精确单体重复单元比例的聚合物时，用户需勾选 **强制浓度** 

2D建模
------------------------------------------------------
为了寻找更便捷、更高效的聚合物建模方法，AuToFF上线了聚合物2D建模功能，用户仅需两步即可建立想要的聚合物结果，分别为：首先，跳转至marvin网站绘制聚合物结构；然后，复制mrv格式字符串到下方文本输入框生成3D结构

.. figure:: image/2D建模.png
    :align: center

.. note::

 marvin网站绘制2d结构确定重复单元时一定要框选所有的原子，将所有原子包含进去，不能出现红点标记，否则将会识别读取有误

.. figure:: image/marvin.png
    :align: center