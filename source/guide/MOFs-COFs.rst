.. _MOFs-COFs:

金属/共轭有机框架
================================================

目前本模块搭载了金属/共轭有机框架结构数据库，1）金属有机框架结构库:即MOFs材料，包含九种数据库，如CPO-27、IRMOF、M2、MIL、PCN、ZIF、CoRE、UiO等。2）共轭有机框架结构库:即COFs材料，包含CURATED-COFs和CoRE-COF_DT613-v6.0两类数据库。或者通过用户上传cif、vasp、gjf、car文件格式导入相应结构。支持全原子力场以及针对MOFs/COFs材料特定力场，如UFF4MOF、UFF4MOFII、ZIF-FF、ZIF-8等。


.. figure:: image/MOF-COF-选择晶体文件.png
    :align: center
.. centered:: 图5.1  金属/共轭有机框架界面

其中 **根据力场选择原子类型** 界面的电荷计算方法，其中包括Eqeq、EEM、GMP-Qeq、MEPO-Qeq、m-CBAC，还额外增加了高精度的机器学习预测MOF和COF材料的REPEAT和DDEC电荷，即packmof-MOF-REPEAT、packmof-COF-DDEC。


.. figure:: image/MOF-COF-电荷类型.png
    :align: center
.. centered:: 图5.1  金属/共轭有机框架——选择电荷类型