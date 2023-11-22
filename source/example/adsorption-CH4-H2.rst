.. _adsorption-CH4-H2:

沸石分子筛的甲烷/氢气吸附选择性模拟
=============================================================
选择吸附通过对二氧化硅和氧化铝极有规律的连接，形成的沸石分子筛的微孔具有均匀性，使得其吸附具有选择性。选择吸附通过对二氧化硅和氧化铝极有规律的连接，形成的沸石分子筛的微孔具有均匀性，使得其吸附具有选择性。下例我们通过GCMC方法研究了ASV沸石对 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 的二元混合组份吸附，计算不同组分的混合气体体系中对 :math:`\ce{CH_4}` 气体分子的选择吸附性能。


力场辅助工具-AuToFF
-------------------------

确定起始构型
#########################

选择AuToFF-晶体材料模块，选择沸石 **zeolites** 文件夹- **ASV.cif** 完成结构建模，点击 **生成3d结构视图** 按钮即可3D显示，继而选择 **TraPPE-zeo** 力场开始进行力场参数分配。

.. figure:: image/adsorption-RASPA/选择晶体文件.png
    :align: center
.. centered:: 图3.8.1  选择ASV晶体文件

由于 :math:`\ce{CH_4}` 分子将采用联合原子力场，因此直接在联合原子力场模块界面进行模型搭建。

.. figure:: image/adsorption-RASPA/选择CH4分子.png
    :align: center
.. centered:: 图3.8.2  创建 :math:`\ce{CH_4}` 分子结构


根据力场选择原子类型
#####################
TraPPE-zeo是特定针对沸石骨架体系的力场。

.. figure:: image/adsorption-RASPA/根据力场选择原子类型.png
    :align: center
.. centered:: 图3.8.3  ASV晶体根据力场选择原子类型

对于  :math:`\ce{CH_4}`  分子, 采用联合原子势能模型，即TraPPE-UA。

.. figure:: image/adsorption-RASPA/CH4根据力场选择原子类型.png
    :align: center
.. centered:: 图3.8.4   :math:`\ce{CH_4}` 分子根据力场选择原子类型

生成拓扑文件
#####################
根据力场的选择即可生成拓扑文件的相关力场参数，Lennard-Jones (LJ)势能函数和Coulomb势能函数共同描述吸附质分子之间以及客体分子与沸石骨架原子的非键相互作用。选择RASPA软件即可下载对应软件格式的力场输入文件和结构文件。

.. figure:: image/adsorption-RASPA/生成拓扑文件.png
    :align: center
.. centered:: 图3.8.5  生成ASV沸石分子筛晶体的拓扑文件

.. figure:: image/adsorption-RASPA/CH4生成拓扑文件.png
    :align: center
.. centered:: 图3.8.6  生成 :math:`\ce{CH_4}` 分子的拓扑文件


GCMC模拟-RASPA
-------------------------
GCMC模拟了温度为 300 K 时沸石分子筛ASV晶体在  :math:`\ce{CH_4}` / :math:`\ce{H_2}`  混合体系中对 :math:`\ce{CH_4}` 气体分子的吸附选择性能，具体运行所需文件详见如下：

simulation.input 输入文件
##########################################
包含模拟类型, 模拟的步数, 骨架名字, 晶胞数目, 使用的小分子, Monte-Carlo 行动 (move) 类型等控制词条。 

.. code-block::

     SimulationType                MonteCarlo
     NumberOfCycles                25000
     NumberOfInitializationCycles  2000
     PrintEvery                    1000
     
     #ContinueAfterCrash            no
     #WriteBinaryRestartFileEvery   2000
     
     Forcefield                    local
     RemoveAtomNumberCodeFromLabel yes
     
     Framework 0
     FrameworkName asv
     UnitCells 3 3 2
     HeliumVoidFraction 0.21
     ExternalTemperature 300.0
     ExternalPressure 1e5
     
     
     Component 0 MoleculeName               ch4
                 MoleculeDefinition         local
                 MolFraction                0.5        #摩尔分数
                 FugacityCoefficient        1.0
                 TranslationProbability     0.5        #平移概率
                 RegrowProbability          0.5        #重生概率
                 IdentityChangeProbability  1.0        #改变身份概率
                   NumberOfIdentityChanges  2          #身份改变次数
                   IdentityChangesList      0 1        #身份互换组分列表
                 SwapProbability            1.0        #交换概率
                 CreateNumberOfMolecules    0          #交换概率
     
     Component 1 MoleculeName               H2
                 MoleculeDefinition         local
                 MolFraction                0.5
                 FugacityCoefficient        1.0
                 TranslationProbability     0.5
                 RegrowProbability          0.5
                 IdentityChangeProbability  1.0
                   NumberOfIdentityChanges  2
                   IdentityChangesList      0 1
                 SwapProbability            1.0
                 CreateNumberOfMolecules    0

    

structure-name.cif 结构文件
##########################################
多孔材料的结构文件，AuToFF中下载的压缩包中包含结构文件，ASV沸石骨架结构下载链接 :download:`asv.cif <files/asv.cif>`


pseudo_atoms.def 结构文件
##########################################
列举使用的赝原子的信息，包括电荷和质量等。一般情况下赝原子代表一个原子，但也可能代表一个小基团 (比如 -CH3)。由于 CIF 文件会提供原子信息，因此在 CIF 中列举的原子并不需要在赝原子列表中进行规定，当读取 CIF 文件时原子信息将自动加入到该列表中。如果在赝原子中也提供了原子信息，那么该文件中的数据将被优先读取。
AuToFF分别下载 :math:`\ce{CH_4}` 分子和ASV沸石的拓扑文件，两个文件夹中的pseudo_atoms.def以及 :math:`\ce{H_2}` 的赝势原子参数进行整合，内容如下：

.. code-block::

     #number of pseudo atoms. Created by AutoFF
     5
     # type print as scat oxidation mass charge polarization B-factor radii connectivity anisotropic anisotropic-type tinker-type
       zeo_Si yes   Si  Si 0    28.085499    1.500000 0.0 1.0 1.0 0 0.0 relative 0
       zeo_OZ yes    O   O 0    15.999405   -0.750000 0.0 1.0 1.0 0 0.0 relative 0
       Tra_CH4 yes  CH4 CH4 0   16.043000    0.000000 0.0 1.0 1.0 0 0.0 relative 0
       H_h2  yes     H   H  0   1.00794     0.468    0.0  1.0 0.7 0 0   relative 0
       H_com  no     H   H  0   0.0        -0.936    0.0  1.0 0.7 0 0   relative 0


.. note:: 

    * 混合体系中 :math:`\ce{H_2}` 分子采用 Darkrim 和 Levesque 开发的三点模型, 在双原子分子的质心 (center of mass, COM) 引入点电荷来重现实验中分子的四极矩现象。
    * pseudo_atoms.def中的H相关参数详见RASPA2-master/forcefield/GenericZeolites/pseudo_atoms.def

force_field_mxing_rules.def 力场文件
##########################################
定义每个原子的势参数和混合规则

.. code-block::

     # general rule for shifted vs truncated. Created by AutoFF
     shift
     # general rule tail corrections
     no
     # number of defined interactions
     6
     # type interaction, parameters. IMPORTANT: define generic matches first
       Si  lennard-jones    21.999858    2.300000
       O  lennard-jones    52.999673    3.300000
      Tra_CH4  lennard-jones   147.999944    3.730000
       H_h2    lennard-jones    36.7      2.958
       H_com   none
       H_h2    lennard-jones    36.7      2.958
     # general mixing rule for Lennard-Jones
     Lorentz-Berthelot

.. note:: 

    * 为了降低计算量，输入文件RemoveAtomNumberCodeFromLabel变量设置了yes参数，意味着在读取cif格式地Framework信息时，将元素后面的序号都删除，因此force_field_mxing_rules.def文件中原子类型仅需修改成Si，O
    * force_field_mxing_rules.def中的H参数详见RASPA2-master/forcefield/GenericZeolites/force_field_mixing_rules.def

Framework.def 文件
##########################################
Framework.def存储骨架结构键, 键角, 二面角的伸缩扭转等参数 (非必须) ，AuToFF中下载的压缩包中包含该文件，下载链接 :download:`asv.def <files/asv.def>`


molecules.def 分子文件
##########################################

由于simulation.input输入文件定义MoleculeDefinition参数为local，需在该目录存放该分子结构信息文件，即 :download:`H2.def <files/H2.def>`  、 :download:`ch4.def <files/ch4.def>`

.. note:: 

    * H2.def详见RASPA2-master/molecules/TraPPE/H2.def

结果分析
-------------------------
在模拟 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 分离过程中, 二元混合组份的吸附选择性系数计算公式如下:

.. math:: 
   S = \frac{x_{A}y_{B}}{x_{B}y_{A}}

300 K下、100 kPa时，超微孔沸石材料ASV在组分不同的 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 混合体系中对 :math:`\ce{CH_4}` 组份吸附选择性能模拟，结果如下：

.. figure:: image/adsorption-RASPA/吸附质进料比对沸石ASV的ch4选择性的影响.png
    :align: center
.. centered:: 图3.8.7  吸附质分子进料比对沸石ASV的 :math:`\ce{CH_4}` 选择性的影响

结果可见，吸附质分子的进料比对甲烷分子的分离性能影响比较小, 这也表现出ASV材料在不同环境下具有筛选甲烷分子的优良潜质。 


300 K下时，模拟体系不同体相压力下，沸石ASV在等摩尔 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 混合体系中对 :math:`\ce{CH_4}` 的吸附选择性，结果如下：

.. figure:: image/adsorption-RASPA/压力对沸石ASV的ch4选择性的影响.png
    :align: center
.. centered:: 图3.8.7  压力对沸石ASV的 :math:`\ce{CH_4}` 选择性的影响


由此可见，在 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 系统中沸石ASV对 :math:`\ce{CH_4}` 的吸附选择性几乎与体相压力的变化无关。这说明沸石的结构特征直接决定了 :math:`\ce{H_2}` 气体分子的吸附选择性。

