.. _IL:

离子液体
================================================

目前支持的离子液体相关力场包括CL&P、OPLS-2009IL、Merz和Haiyang Zhang开发针对不同的水模型体系下的金属离子的LJ势函数。具体包括有CL&P、OPLS-2009IL、OPLS-2009IL/0.8、OPLS-IONS_2006、Madrid-2019、Merz/IOD/OPC、Merz/IOD/OPC3、Merz/IOD/SPCE、Merz/IOD/TIP3P、Merz/IOD/TIP3P-FB、Merz/IOD/TIP4P-Ew、Merz/IOD/TIP4P-FB、Merz/IOD/All_TIP3、Merz/HFE/OPC、Merz/HFE/OPC3、Merz/HFE/SPCE、Merz/HFE/TIP3P、Merz/HFE/TIP3P-FB、Merz/HFE/TIP4P-Ew、Merz/HFE/TIP4P-FB、Merz/CM/OPC、Merz/CM/OPC3、Merz/CM/SPCEMerz/CM/TIP3P、Merz/CM/TIP3P-FB、Merz/CM/TIP4P-EwMerz/CM/TIP4P-FB、HaiyangZhang/a99SB-disp、HaiyangZhang/OPC、HaiyangZhang/OPC3、HaiyangZhang/SPCE、HaiyangZhang/SPCEb、HaiyangZhang/TIP3P、HaiyangZhang/TIP3P-FB、HaiyangZhang/TIP4P-2005、HaiyangZhang/TIP4P-D、HaiyangZhang/TIP4P-Ew、HaiyangZhang/TIP4P-FB、SMALL/OH-、CRYSTAL/CaCO3、PolyOxoMetalates/VOx、PolyOxoMetalates/FeCl4、PolyOxoMetalates/Uranyl/GW。

首先是，创建分子结构，包含三种方式，同全原子力场操作。

.. figure:: image/IL-FF-创建分子结构.png
    :align: center
.. centered:: 图3.1  创建分子结构

第二，根据力场选择原子类型，自动匹配原子类型。

.. figure:: image/IL-FF-根据力场选择原子类型.png
    :align: center
.. centered:: 图3.2  根据力场选择原子类型

第三，生成对应的力场参数，产生力场拓扑文件。
    
.. figure:: image/IL-FF-生成拓扑文件.png
    :align: center
.. centered:: 图3.3  生成拓扑文件