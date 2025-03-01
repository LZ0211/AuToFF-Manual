.. _Minerals:

晶体材料
================================================

晶体材料是一类周期性结构，内部原子、离子或分子在三维空间内按照一定的规律排列，具有长程有序性。目前本模块搭载了多种晶体结构数据库，方便用户直接导入结构模型，包括有机分子晶体、锑化物合金、金属互化物、沸石、陶瓷、氧化物、硫化物、硫酸盐、氮化物、碳化物、碳酸盐、卤化物、氢氧化物、粘土等。也可通过用户上传cif、vasp、gjf、car文件格式导入相应结构。此外，针对晶体材料支持的力场包括ClayFF、CVFF/harmonic、CVFF/morse、CVFF_aug/harmonic、CVFF_aug/morse、Interface/PCFF、Interface/CHARMM、FCC-Metal、Metal、TraPPE-zeo、CRYSTAL/Silica、CRYSTAL/TiO2、CRYSTAL/CaCO3、PCFF、CFF91、COMPASS。



.. figure:: image/Minerals-选择晶体文件.png
    :align: center
.. centered:: 图6.1  晶体材料模块界面

.. figure:: image/Minerals-晶体材料结构库.png
    :align: center
.. centered:: 图6.2  晶体材料结构库

.. note::

    1. COMPASS力场是由上海交通大学孙淮教授课题组开发的第一个出自量子力学从头计算的新一代力场。比较CVFF、PCFF以及Universal等其他的力场，COMPASS力场的原子信息，能量和尺度参数等较全，可以应用于有机分子、无机分子和聚合物等多类复杂体系，甚至能准确得到凝聚态体系的物理图景和各种宏观性质，使模拟计算具有高精度的预测能力。使用该力场可以在很宽的温度和压力区间内准确得到体系中各粒子的振动情况及热物理性质，也可以用来研究具有表面或存在共混现象的比较复杂的体系，这是其他力场难以实现的。
    2. PCFF力场更适用于聚合物一类的有机材料，以及包含二十种金属，也可运用在糖类、脂类和核酸体系中。
    3. CVFF 力场又称一致共价力场，更适用于计算蛋白质，多肽等生化大分子有机体系。