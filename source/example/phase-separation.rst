.. _phase-separation:

正辛醇和水相分离的MD模拟
================================================

正辛醇则是一种长链脂肪醇，分子中带有8个碳原子和一个氧原子。由于分子结构中没有明显的极性键或官能团，正辛醇分子整体带有较低的极性。因此，正辛醇与水分子之间的相互作用力较弱，而水分子与水分子之间的相互作用力比较强，因此正辛醇只能微溶于水，会发生相分离现象。本案例通过联合原子力场建立正辛醇分子模型，选择OPC3力场建立水分子模型，继而进行分子动力学模拟相分离现象。其中AuToFF支持联合原子力场建立模型，其中联合原子力场是指与碳原子直接键连的氢原子的相对原子质量被重叠到碳原子上，形成一个联合原子的整体，同时，其他原子对氢原子的相互作用也被叠加到联合原子上，从而可以大幅度奖励力场的复杂性，减少势参数。在联合原子力场中，大多数有机分子的力点数约为原子数的1/3（平均为CH2）,MD模拟的计算效率大约可以提高一个数量级。

力场辅助工具-AuToFF
-------------------------------------------------------

确定起始构型
########################################################


选择AuToFF-联合原子力场模块，首先通过2D分子建模建立正辛醇结构简式，点击“生成3D结构视图”按钮即可3D显示，力场选择TraPPE联合原子力场。

.. figure:: image/example-phase-separation/确定正辛醇结构.PNG
    :align: center
.. centered:: 图3.4.1  确定正辛醇结构


选择力场
########################################################

正辛醇采用联合原子力场-TraPPE力场，AuToFF程序会根据分子自动匹配相应的原子类型

.. figure:: image/example-phase-separation/根据力场选择原子类型.png
    :align: center
.. centered:: 图3.4.2  根据力场选择原子类型


生成拓扑文件
########################################################

根据力场的选择即可生成拓扑文件（top文件）的相关力场参数（itp文件，包括LJ、键、键角参数，原子电荷）。此外生成拓扑文件可支持多款计算软件，包括：GROMACS、LAMMPS、AMBER、Moltemplate、OpenMM、TINKER、CHARMM。

.. figure:: image/example-phase-separation/生成正辛醇的力场拓扑文件.PNG
    :align: center
.. centered:: 图3.4.3  生成正辛醇的力场拓扑文件


同理，对于水分子的力场拓扑文件生成选择全原子力场模块，力场选择OPC3即可，其余步骤同上

.. figure:: image/example-phase-separation/确定水分子的力场类型.PNG
    :align: center
.. centered:: 图3.4.4  确定水分子的力场类型

模拟体系建模
-------------------------------------------------------

构建体系
########################################################

首先，创建模拟体系。通过Packmol软件，我们将正辛醇分子和水分子放入一个立方体的模拟盒子中。这个过程中立方体的盒子大小要略大于同等密度下混合体系所需要的体积，以保证有足够的空间使得溶剂分子能够随机的分布并且模拟可以快速平衡。将AuToFF创建并下载好每个组分的拓扑文件，然后把pdb文件拷贝到packmol文件夹，调用packmol程序生成模拟的盒子。Packmol输入文件model.inp如下：

.. code-block::
  
   tolerance 2.0
   filetype pdb
   add_box_sides 1.5
   output model.pdb
     structure Oct.pdb
       number  500
         inside cube 0. 0. 0. 80
     end structure
     structure h2o.pdb
         number 4000
         inside cube 0. 0. 0. 80
     end structure

运行 packmol < model.inp 可生成model.pdb文件，该文件包含了正辛醇和水混合模拟体系中所有原子的坐标，但缺少键、键角等拓扑结构信息。将得到的model.pdb导入到VMD显示如下

.. figure:: image/example-phase-separation/正辛醇和水混合体系初始构型.jpg
    :align: center
.. centered:: 图3.4.5  正辛醇和水混合体系初始构型

构建拓扑文件
########################################################

拓扑文件是gromacs运行模拟所必需的文件，它提供了模拟体系中所有分子的拓扑结构、力场文件的引用、约束力参数……；拓扑文件必须包含三个层次：

- 参数级别；这一部分包括了力场设定
- 分子定义级别：这一部分包含了一个或多个分子对应的.itp文件。实际上，.itp 文件可以看做是 .top 文件分子定义级别（针对每单个分子）单独拿出储存的信息，他们形成了一个嵌套式的引用关系
- 体系级别：只包含体系的特定信息

正辛醇和水混合模拟体系的top文件model.top如下：

.. code-block:: 

   #define _FF_OPLS
   #define _FF_OPLSAA
   [ defaults ]
   1 3 yes 0.5 0.5
   #include "Oct_ATP.itp"
   #include "h2o_ATP.itp"
   #include "Oct.itp"
   #include "h2o.itp"
   [ system ]
   500Oct+4000h2o
   [ molecules ]
   Oct      500
   h2o      4000

MD模拟
-------------------------------------------------------

在模拟过程中，模拟步长设为２fs，积分算法选择速度Verlet算法。模拟体系的三个方向均考虑周期性，是体相的模拟。正辛醇-水混合体系的相分离过程模拟，采用梯度退火模拟。具体流程如下：等温等压系综下，模拟体系首先被缓慢加热到330 K，然后逐步将温度下降至目标温度298.15 K 。使用V-rescale控温，参考温度298.15 K, Berendsen控压， 参考压力为 1.01325 bar 。完整的GROMACS的mdp文件输入如下：

.. code-block:: 

   define =
   integrator = md-vv-avek
   
   
   dt         = 0.002
   nsteps     = 2000000
   comm-grps  = system
   energygrps =
   ;
   nstxout = 0
   nstvout = 0
   nstfout = 0
   nstlog  = 500
   nstenergy = 500
   nstxout-compressed = 1000
   compressed-x-grps  = system
   ;
   annealing = single
   annealing_npoints = 3
   annealing_time = 0 2000 4000
   annealing_temp = 0 330 298.15
   ;
   pbc = xyz
   cutoff-scheme = Verlet
   coulombtype   = PME
   rcoulomb      = 1.0
   vdwtype       = cut-off
   rvdw          = 1.0
   DispCorr      = EnerPres
   ;
   Tcoupl  = V-rescale
   tau_t   = 0.5
   tc_grps = system
   ref_t   = 298.15
   ;
   Pcoupl     = Berendsen
   pcoupltype = isotropic
   tau_p = 1
   ref_p = 1.01325
   compressibility = 8.5e-5
   ;
   
   gen_vel  = no
   gen_temp = 298.15
   gen_seed = -1
   ;
   freezegrps  =
   freezedim   =
   constraints = hbonds


MD结果分析
-------------------------------------------------------


MD过程的轨迹变化通过VMD作图如下,可以清晰的展现出水-正辛醇自发相分离现象，如下图所示，初始构型是混合体系，正辛醇和水均匀混合在一起，进行一段时间模拟后正辛醇和水相互分离开，并且正辛醇形成类似磷脂双层膜的结构，亲水的羟基头部朝水，而疏水的烃链尾巴朝内。

.. figure:: image/example-phase-separation/正辛醇和水混合体系相分离模拟轨迹变化.gif
    :align: center
.. centered:: 图3.4.6  正辛醇和水混合体系相分离模拟轨迹变化