.. _AA-FF:

全原子力场
================================================

全原子力场 (All-atom force fields) 中，体系的力点与分子中的全部原子一一对应，质量集中在原子核上。也就是说，力点与原子核的位置或原子的质心位置重合。简单来说，全原子力场中，分子由其组成原子为质点的集合构成。全原子力场的优点是具有更高精确度，可以更好地描述生物分子体系的结构与性质。但是，全原子力场的计算量大，模拟效率低。因此，在计算资源受到限制时，模拟者必须减少模拟体系的规模或缩短模拟体系的实际演化时间，影响模拟效果。


创建分子结构
-------------------------------------------------------
AuToFF支持2D建模和3D分子显示，用户可以通过2D建模的方式建立自己所期望的分子构型。同时，也支持多种结构文件(.mol/.pdb/.xyz/.mol2)的导入识别。


.. figure:: image/AA-FF-创建分子结构.png
    :align: center
.. centered:: 图1.1  创建分子结构

AuToFF也内置了分子结构库（电解液分子、水分子、维生素分子、碳水化合物、酶、氨基酸分子等）。

.. figure:: image/选取分子结构.png
    :align: center
.. centered:: 图1.2  选取分子结构

用户在下拉框选择 **选取分子结构** 即可进入分子结构数据库选择常见分子。

.. figure:: image/分子结构库.png
    :align: center
.. centered:: 图1.3  分子结构库

此外，也可通过SMILES字符串解析小分子化学结构并建模。点击 **import图标** 即可进入SMILES输入框，输入SMILES字符串,继而自动生成3D分子结构

.. figure:: image/SMILES图标.png
    :align: center
.. centered:: 图1.4  SMILES图标

.. figure:: image/SMILES输入框.png
    :align: center
.. centered:: 图1.5  SMILES输入框

根据力场选择原子类型
-------------------------------------------------------
目前支持的全原子力场包括GAFF、GAFF2、OPLS-AA、CGenFF、CVFF......，通用力场包括UFF、DREIDING。此外，针对特定的水模型支持多种力场函数类型表示，包括OPC、OPC3、SPC、SPC/E、SPC/Eb、TIP3P、TIPS3P、TIP3P-FB、TIP4P、TIP4P/Ew、TIP4P/2005、TIP4P/ICE、TIP4P/epsilon、SPC/E-HW、TIP3P-HW、TIP4P/2005-HW。用户也可选择多种charge类型，包括普适原子电荷1.14*CM1A、1.14*CM1A-LBCC、AM1BCC、MMFF94、XTB-RESP、CM5、1.2XCM5、QEq和AI电荷GNN-RESP等。

若用户利用量化优化后的结构进行力场拓扑文件生成，想用该结构的键长键角参数，即可选中左下角 **使用当前结构的键长/键角** 按钮，程序将自动保留原始输入结构的键长键角参数。

此外，AuToFF可实现mSeminario方法进行力场参数拟合，点击 **力场参数拟合** 按钮，用户仅需上传量化计算得到hessian文件即可进行参数拟合，hessian文件可支持BDF，gaussian，orca，XTB软件格式。


.. figure:: image/力场参数拟合.png
    :align: center
.. centered:: 图1.6  力场参数拟合

.. figure:: image/AA-FF-根据力场选择原子类型.png
    :align: center
.. centered:: 图1.7  根据力场选择原子类型

.. note::

    1. OPLS力场的势函数被严格限制于最常见的势函数形式：共价键伸缩势和键角弯曲势采用谐振子势函数，二面角扭曲势只包括Fourier展开式的前三项，van der Waals相互作用采用Lennard-Jones 12-6 势函数，静电相互作用采用库伦是函数。同时，所有力点位于原子（核）上，不考虑力点偏离原子中心。在计算分子内非键相互作用时，完全排除1-2和1-3相互作用，但只排除50%的1-4相互作用。计算交叉项的van der Waals相互作用时，采用Lorentz-Berthelot混合规则计算Lennard-Jones势函数。
    2. 在描写水分子的力场中，TIP3P是全原子力场，质点、力点、电荷点重合。相反，TIP4P、TIP5P等，力点的数目超过质点的数目或原子数。选择力点与分子中各个原子核的位置重叠，可以在模拟中省去重新分布受力和力矩的计算。
    3. 在模拟生物分子的水溶液时，OPLS力场应与TIP4P或TIP3P水分子模型搭配，可以取得更好的模拟效果。
    4. CGenFF力场是CHARMM的通用力场，多了一项Urey-Bradley相互作用势，以弥补键角弯曲势的不足。
    5. 在DREIDING力场中，参与分子内或分子间相互作用的所有原子，都被按其成键的杂化类型或几何构型进行系统分类，同种类型的原子参与相互作用性质相同，不同种类型的原子参与相互作用性质不同。DREIDING力场在计算结合能和分子相互作用时的精度更高，但一些过渡金属元素相关参数存在缺失。 
    6. UFF力场比DREIDING力场有更广泛的适用范围，是对DREIDING力场的发展。
    7. GNN-RESP电荷是基于机器学习的第一性原理精度的RESP原子电荷。


生成拓扑文件
-------------------------------------------------------
用户建立完分子结构，选择相应的力场，进而生成拓扑文件，也可进行力场参数的修改。最后，选择计算软件从而生成相应的输入文件，支持GROMACS、LAMMPS、AMBER、MOLTEMPLATE、OPENMM、TINKER、CHARMM、GEAR、RASPA、NAMD、GOMC、GAUSSIAN、ORCA软件格式输出，其中GAUSSIAN、ORCA格式为QM/MM输入参数生成，若想生成BDF软件的QM/MM输入参数生成，用户选择AMBER软件下载拓扑文件，格式即可兼容。AuToFF可以有效帮助用户解决力场的选择、参数的生成、复杂体系的建模等多种分子模拟过程种遇到的困难，AuToFF可以有效降低这些使用门槛，可以极大的扩大分子动力学模拟的用户群体。


.. figure:: image/AA-FF-生成拓扑文件.png
    :align: center
.. centered:: 图1.8  生成拓扑文件

.. note::
 
 AuToFF程序所提供的力场单位说明如下：
 
    1. 针对分子间相互作用力参数中的范德华半径单位是nm，势阱深度单位是kJ/mol
    2. 针对键伸缩能中参数1单位是nm，参数2单位是kJ/mol/nm^2
