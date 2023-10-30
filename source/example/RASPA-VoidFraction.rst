.. _RASPA-VoidFraction:

RASPA计算孔隙率
=========================================
孔隙率是描述分子筛内部空隙含量的重要参数。沸石分子筛具有非常高的孔隙率，大的内表面积和约50％体积的空腔。在高温活化沸石失水后，在晶体内部形成大量具有均匀孔径的孔，其具有强吸附能力并且可以有效地将直径小于其孔径的分子吸入孔中，并且大于孔径的分子被阻挡在孔外，从而分离出不同大小的分子。可利用RASPA计算孔隙率，以氦气作为探针。具体操作详见如下。

力场辅助工具-AuToFF
-------------------------

确定起始构型
#########################

选择AuToFF-晶体材料模块，选择沸石 **zeolites** 文件夹- **ASV.cif** 完成结构建模，点击 **生成3d结构视图** 按钮即可3D显示，继而选择 **TraPPE-zeo** 力场开始进行力场参数分配。

.. figure:: image/adsorption-RASPA/选择晶体文件.png
    :align: center
.. centered:: 图3.6.1  选择ASV晶体文件


根据力场选择原子类型
#####################
TraPPE-zeo是特定针对沸石骨架体系的力场。

.. figure:: image/adsorption-RASPA/根据力场选择原子类型.png
    :align: center
.. centered:: 图3.6.3  ASV晶体根据力场选择原子类型


生成拓扑文件
#####################
根据力场的选择即可生成拓扑文件的相关力场参数，Lennard-Jones (LJ)势能函数和Coulomb势能函数共同描述吸附质分子之间以及客体分子与沸石骨架原子的非键相互作用。选择RASPA软件即可下载对应软件格式的力场输入文件和结构文件。

.. figure:: image/adsorption-RASPA/生成拓扑文件.png
    :align: center
.. centered:: 图3.6.5  生成ASV沸石分子筛晶体的拓扑文件


MC模拟-RASPA
-------------------------
采用MC模拟方法以氦气作为探针，计算ASV沸石结构的孔隙率，具体运行所需文件详见如下：

simulation.input 输入文件
##########################################
包含模拟类型, 模拟的步数, 骨架名字, 晶胞数目, 使用的小分子, Monte-Carlo 行动 (move) 类型等控制词条。 

.. code-block::

    SimulationType        MonteCarlo
    NumberOfCycles        10000
    PrintEvery            1000
    PrintPropertiesEvery  1000
    
    Forcefield            local
    RemoveAtomNumberCodeFromLabel yes
    
    Framework 0
    FrameworkName asv
    UnitCells 3 3 2
    ExternalTemperature 298.0
    
    Component 0 MoleculeName             helium
                MoleculeDefinition       local
                WidomProbability         1.0
                CreateNumberOfMolecules  0
    

structure-name.cif 结构文件
##########################################
多孔材料的结构文件，AuToFF中下载的压缩包中包含结构文件，ASV沸石骨架结构下载链接 :download:`asv.cif <files/asv.cif>`


pseudo_atoms.def 结构文件
##########################################
列举使用的赝原子的信息，包括电荷和质量等。一般情况下赝原子代表一个原子，但也可能代表一个小基团 (比如 -CH3)。由于 CIF 文件会提供原子信息，因此在 CIF 中列举的原子并不需要在赝原子列表中进行规定，当读取 CIF 文件时原子信息将自动加入到该列表中。如果在赝原子中也提供了原子信息，那么该文件中的数据将被优先读取。
AuToFF下载ASV沸石的拓扑文件，对ASV晶体和氦气分子的pseudo_atoms.def进行整合，内容如下：

.. code-block::

   #number of pseudo atoms. Created by AutoFF
   3
   # type print as scat oxidation mass charge polarization B-factor radii connectivity anisotropic anisotropic-type tinker-type
     zeo_Si yes   Si  Si 0    28.085499    1.500000 0.0 1.0 1.0 0 0.0 relative 0
     zeo_OZ yes    O   O 0    15.999405   -0.750000 0.0 1.0 1.0 0 0.0 relative 0
     He     yes   He  He 0    4.002602     0.0      0.0 1.0 1.0 0 0.0 relative 0

.. note:: 

    * pseudo_atoms.def中的He参数详见RASPA2-master/forcefield/GarciaPerez2006/pseudo_atoms.def

force_field_mxing_rules.def 力场文件
##########################################
定义每个原子的势参数和混合规则

.. code-block::

    # general rule for shifted vs truncated. Created by AutoFF
    shift
    # general rule tail corrections
    no
    # number of defined interactions
    3
    # type interaction, parameters. IMPORTANT: define generic matches first
      Si  lennard-jones    21.999858    2.300000
      O  lennard-jones    52.999673    3.300000
      He lennard-jones    10.9      2.64
    # general mixing rule for Lennard-Jones
    Lorentz-Berthelot

.. note:: 

    * 为了降低计算量，输入文件设置了RemoveAtomNumberCodeFromLabel参数，因此force_field_mxing_rules.def文件中原子类型仅需修改成Si，O
    * force_field_mxing_rules.def中的He参数详见RASPA2-master/forcefield/GarciaPerez2006/force_field_mixing_rules.def

Framework.def 文件
##########################################
Framework.def存储骨架结构键, 键角, 二面角的伸缩扭转等参数 (非必须) ，AuToFF中下载的压缩包中包含该文件，下载链接 :download:`asv.def <files/asv.def>`


molecules.def 分子文件
##########################################

由于simulation.input输入文件定义MoleculeDefinition参数为local，需在该目录存放该分子结构信息文件，即 :download:`helium.def <files/helium.def>`

结果分析
-------------------------

输出文件即可查看计算模拟所得ASV沸石分子筛的孔隙率，即为0.21

.. code-block::


     Average Widom Rosenbluth factor:
     ================================
             Block[ 0] 0.213241 [-]
             Block[ 1] 0.212941 [-]
             Block[ 2] 0.211396 [-]
             Block[ 3] 0.21113 [-]
             Block[ 4] 0.210937 [-]
             ------------------------------------------------------------------------------
             [helium] Average Widom Rosenbluth-weight:   0.211929 +/- 0.001929 [-]



