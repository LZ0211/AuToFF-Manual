.. _adsorption-CH4-H2:

沸石分子筛的甲烷/氢气吸附选择性模拟
=============================================================
分子筛作为一种重要的材料，在气体处理与分离方面具有广泛的应用潜力。气体处理与分离是工业、环境和能源领域中非常重要的研究领域。其中，重要的应用包括天然气加工、石油化工、空气净化、环境污染治理等。在这些应用中，气体处理与分离面临着许多挑战，例如：

* 气体混合物的复杂性：气体混合物通常由多种组分组成，包括不同的气体分子、杂质、水蒸汽等。这些组分之间的相互作用和竞争性吸附会影响到分离的效率和选择性。

* 目标组分的低浓度：在一些应用中，需要从低浓度的气体混合物中分离出目标组分。例如，从二氧化碳含量较低的气流中提取二氧化碳，需要具有高效的选择性吸附材料。

* 处理规模的不同：气体处理与分离的规模从小到大，从个人使用到工业生产都有涉及。因此，需要针对不同规模的处理应用，开发不同的材料和设备。

* 能源消耗：气体处理与分离通常需要大量的能源投入，例如吸附、膜分离等过程需要在高压下进行，消耗大量的电力和气体。

以上是气体处理与分离领域面临的挑战，解决这些挑战需要不断地发展新材料和技术，并结合实际应用需求进行研究。分子筛在气体处理与分离领域具有广泛的应用研究领域。以下是其中一些关键的应用研究领域：

* 气体吸附分离：分子筛可以通过选择性吸附不同气体组分来实现气体的分离。例如，通过调节分子筛的孔径和表面性质，可以实现二氧化碳的捕获与封存、甲烷的选择性吸附等。此外，分子筛还可用于空气中的有害气体去除，如去除甲醛、苯等有机污染物。

* 催化氧化：分子筛被广泛应用于催化反应中，其中包括气体处理与分离领域。例如，在汽车尾气处理中，分子筛可以作为催化剂的载体，通过催化氧化将有害气体转化为无害物质，如将一氧化碳转化为二氧化碳。

* 脱硫脱氮：分子筛可以用于脱除燃烧废气中的硫氧化物和氮氧化物。通过选择性吸附和催化反应，分子筛可以将硫氧化物和氮氧化物转化为无害物质，从而减少对环境的影响。

* 气体传感：分子筛可以用于气体传感器的制备。通过选择性吸附特定气体，分子筛可以实现对目标气体的高灵敏度检测。这在环境监测、生命安全等领域具有重要应用价值。

这些应用研究领域也充分说明了分子筛在气体处理与分离中的重要性和潜力。通过不断筛选和开发新材料、调节操作条件，可以进一步提高分子筛的性能，拓宽其应用范围，并为解决气体处理与分离问题提供有效的解决方案。

分子筛在气体处理与分离方面的评价指标主要包括以下几个方面：

* 吸附容量：表示吸附剂单位质量或体积对目标组分吸附的能力。通常以吸附剂饱和时目标组分的吸附量来表示。

* 选择性系数：表示吸附剂在混合气流中选择性地吸附某一组分相对于其他组分的能力。通常定义为目标组分和杂质组分吸附量之比。

* 分离因子：表示吸附剂选择性吸附两种组分时，两种组分在吸附剂上的浓度比值与初始混合气流中的浓度比值的比值。分离因子越大，说明分离效果越好。

* 催化活性：表示催化剂对反应物转化的能力。通常定义为反应物转化率与时间的比值。

通过这些评价指标的研究，可以深入了解分子筛的性能和应用特点，并为进一步拓展分子筛在气体处理与分离领域的应用提供支持和指导。

下例我们通过GCMC方法着重探究了ASV沸石对 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 的二元混合组份吸附，计算不同组分的混合气体体系中对 :math:`\ce{CH_4}` 气体分子的选择性系数。


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
.. centered:: 图3.8.8  压力对沸石ASV的 :math:`\ce{CH_4}` 选择性的影响


由此可见，在 :math:`\ce{CH_4}` / :math:`\ce{H_2}` 系统中沸石ASV对 :math:`\ce{CH_4}` 的吸附选择性几乎与体相压力的变化无关。这说明沸石的结构特征直接决定了 :math:`\ce{H_2}` 气体分子的吸附选择性。

