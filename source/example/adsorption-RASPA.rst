.. _adsorption-RASPA:

沸石分子筛吸附 :math:`\ce{CH_4}` 气体的饱和吸附曲线GCMC模拟
=============================================================
沸石是一类重要的多孔晶体材料，它们通过氧桥互相连接形成各种拓扑结构，普遍应用在煤化工、石油化工和精细化工等领域，对  :math:`\ce{CH_4}`  分子有良好的吸附分离性能。因此，本案例我们将以ASV分子筛为例，利用AuToFF建模和生成力场参数文件，进而作为RASPA软件的输入文件进行巨正则蒙特卡罗模拟方法, 获得了沸石分子筛对 :math:`\ce{CH_4}` 气体的吸附等温曲线。


力场辅助工具-AuToFF
-------------------------

确定起始构型
#########################

选择AuToFF-晶体材料模块，选择沸石 **zeolites** 文件夹- **ASV.cif** 完成结构建模，点击 **生成3d结构视图** 按钮即可3D显示，继而选择 **TraPPE-zeo** 力场开始进行力场参数分配。

.. figure:: image/adsorption-RASPA/选择晶体文件.png
    :align: center
.. centered:: 图3.6.1  选择ASV晶体文件

由于 :math:`\ce{CH_4}` 分子将采用联合原子力场，因此直接在联合原子力场模块界面进行模型搭建。

.. figure:: image/adsorption-RASPA/选择CH4分子.png
    :align: center
.. centered:: 图3.6.2  创建 :math:`\ce{CH_4}` 分子结构


根据力场选择原子类型
#####################
TraPPE-zeo是特定针对沸石骨架体系的力场。

.. figure:: image/adsorption-RASPA/根据力场选择原子类型.png
    :align: center
.. centered:: 图3.6.3  ASV晶体根据力场选择原子类型

对于  :math:`\ce{CH_4}`  分子, 采用联合原子势能模型，即TraPPE-UA。

.. figure:: image/adsorption-RASPA/CH4根据力场选择原子类型.png
    :align: center
.. centered:: 图3.6.4   :math:`\ce{CH_4}` 分子根据力场选择原子类型

生成拓扑文件
#####################
根据力场的选择即可生成拓扑文件的相关力场参数，Lennard-Jones (LJ)势能函数和Coulomb势能函数共同描述吸附质分子之间以及客体分子与沸石骨架原子的非键相互作用。选择RASPA软件即可下载对应软件格式的力场输入文件和结构文件。

.. figure:: image/adsorption-RASPA/生成拓扑文件.png
    :align: center
.. centered:: 图3.6.5  生成ASV沸石分子筛晶体的拓扑文件

.. figure:: image/adsorption-RASPA/CH4生成拓扑文件.png
    :align: center
.. centered:: 图3.6.6  生成 :math:`\ce{CH_4}` 分子的拓扑文件

GCMC模拟-RASPA
-------------------------
采用GCMC模拟方法计算ASV沸石结构的甲烷吸附量，具体运行所需文件详见如下：

simulation.input 输入文件
##########################################
包含模拟类型, 模拟的步数, 骨架名字, 晶胞数目, 使用的小分子, Monte-Carlo 行动 (move) 类型等控制词条。 

.. code-block::

   SimulationType                MonteCarlo      #模拟方法为蒙特卡洛方法
   NumberOfCycles                25000           #初始化结束后，用于生成平衡构象(采样)的循环次数。
   NumberOfInitializationCycles  2000            #使用蒙特卡洛法初始化系统所使用的循环次数。
   PrintEvery                    1000            #每隔1000步进行输出打印
    
   Forcefield                    local           #选择当前目录的力场文件，此处直接利用AuToFF一键生成的力场文件
   RemoveAtomNumberCodeFromLabel yes             #在读取cif格式Framework信息时，将元素后面的序号都抹去，极大减少了计算量与分配力场参数的工作量

   Framework 0  
   FrameworkName asv                             #定义多孔材料结构名称
   UnitCells 3 3 2                               #定义晶胞数量3*3*2
   HeliumVoidFraction 0.21                       #定义多孔材料的孔隙率，计算方法见案例7 RASPA计算孔隙率
   ExternalTemperature 300.0                        
   ExternalPressure 1e4
   
   
   Component 0 MoleculeName            ch4       #定义吸附质名称
            MoleculeDefinition         local
            FugacityCoefficient        1.0
            TranslationProbability     0.5       #平移概率
            RegrowProbability          0.5       #重生概率
            SwapProbability            1.0       #交换概率
            CreateNumberOfMolecules    0         #GCMC模拟应始终为0

structure-name.cif 结构文件
##########################################
多孔材料的结构文件，AuToFF中下载的压缩包中包含结构文件，ASV沸石骨架结构下载链接 :download:`asv.cif <files/asv.cif>`


pseudo_atoms.def 结构文件
##########################################
列举使用的赝原子的信息，包括电荷和质量等。一般情况下赝原子代表一个原子，但也可能代表一个小基团 (比如 -CH3)。由于 CIF 文件会提供原子信息，因此在 CIF 中列举的原子并不需要在赝原子列表中进行规定，当读取 CIF 文件时原子信息将自动加入到该列表中。如果在赝原子中也提供了原子信息，那么该文件中的数据将被优先读取。
AuToFF分别下载 :math:`\ce{CH_4}` 分子和ASV沸石的拓扑文件，两个文件夹中的pseudo_atoms.def进行整合，内容如下：

.. code-block::

   #number of pseudo atoms. Created by AutoFF
   3
   # type print as scat oxidation mass charge polarization B-factor radii connectivity anisotropic anisotropic-type tinker-type
     zeo_Si yes   Si  Si 0    28.085499    1.500000 0.0 1.0 1.0 0 0.0 relative 0
     zeo_OZ yes    O   O 0    15.999405   -0.750000 0.0 1.0 1.0 0 0.0 relative 0
    Tra_CH4 yes  CH4 CH4 0    16.043000    0.000000 0.0 1.0 1.0 0 0.0 relative 0



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
     Tra_CH4  lennard-jones   147.999944    3.730000
    # general mixing rule for Lennard-Jones
    Lorentz-Berthelot

.. note:: 

    * 为了降低计算量，输入文件RemoveAtomNumberCodeFromLabel变量设置了yes参数，意味着在读取cif格式地Framework信息时，将元素后面的序号都删除，因此force_field_mxing_rules.def文件中原子类型仅需修改成Si，O。

Framework.def 文件
##########################################
Framework.def存储骨架结构键, 键角, 二面角的伸缩扭转等参数 (非必须) ，AuToFF中下载的压缩包中包含该文件，下载链接 :download:`asv.def <files/asv.def>`


molecules.def 分子文件
##########################################

由于simulation.input输入文件定义MoleculeDefinition参数为local，需在该目录存放该分子结构信息文件，即 :download:`ch4.def <files/ch4.def>`

结果分析
-------------------------

输出文件即可查看计算模拟所得ASV沸石对 :math:`\ce{CH_4}` 的吸附量，即0.0346091881 mol/kg。

.. code-block::

     Number of molecules:
     ====================
     
     Component 0 [ch4]
     -------------------------------------------------------------
             Block[ 0] 13.81640           [-]
             Block[ 1] 13.71180           [-]
             Block[ 2] 13.44740           [-]
             Block[ 3] 13.13800           [-]
             Block[ 4] 13.26120           [-]
             ------------------------------------------------------------------------------
             Average                                     13.4749600000 +/-       0.5158832364 [-]
             Average loading absolute [molecules/unit cell]        0.7486088889 +/-       0.0286601798 [-]
             Average loading absolute [mol/kg framework]          0.0346091881 +/-       0.0013249984 [-]
             Average loading absolute [milligram/gram framework]          0.5552352044 +/-       0.0212569488 [-]
             Average loading absolute [cm^3 (STP)/gr framework]          0.7757295023 +/-       0.0296984812 [-]
             Average loading absolute [cm^3 (STP)/cm^3 framework]          1.4780989560 +/-       0.0565884035 [-]
     
             Block[ 0] 13.81640           [-]
             Block[ 1] 13.71180           [-]
             Block[ 2] 13.44740           [-]
             Block[ 3] 13.13800           [-]
             Block[ 4] 13.26120           [-]
             ------------------------------------------------------------------------------
             Average                                     12.4749600000 +/-       0.5158832364 [-]
             Average loading excess [molecules/unit cell]        0.6930533333 +/-       0.0286601798 [-]
             Average loading excess [mol/kg framework]          0.0320407806 +/-       0.0013249984 [-]
             Average loading excess [milligram/gram framework]          0.5140302432 +/-       0.0212569488 [-]
             Average loading excess [cm^3 (STP)/gr framework]          0.7181612793 +/-       0.0296984812 [-]
             Average loading excess [cm^3 (STP)/cm^3 framework]          1.3684066856 +/-       0.0565884035 [-]



同理，继续通过GCMC方法模拟了超微孔沸石材料 ASV 在300 K下、压力范围为 0—100 kPa 下的 :math:`\ce{CH_4}` 单组份吸附性能，吸附曲线如下：

.. figure:: image/adsorption-RASPA/沸石对CH4的吸附量.png
    :align: center
.. centered:: 图3.6.7  沸石对 :math:`\ce{CH_4}` 的吸附量