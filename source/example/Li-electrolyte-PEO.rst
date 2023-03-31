.. _Li-electrolyte-PEO:

锂离子电池聚合物电解质体系的MD模拟
================================================
锂离子电池因能量密度高、循环稳定性好、无污染等优点已广泛应用于小型电子器件和电动汽车上，并且有望成为未来大型储能系统（EES）的储电供电设备。其中，聚合物电解质具有易加工性、改善锂枝晶生长和提高电池安全性等优点。

力场辅助工具-AuToFF
-------------------------------------------------------

确定起始构型
########################################################
确定双(氟磺酰)亚胺(FSI)阴离子结构，可选择点击AuToFF程序-离子液体模块进行2D建模（如图1），点击“ **生成3d结构视图** ”按钮即可3D显示。

.. figure:: image/example-PEO/创建FSI结构.png
    :align: center
.. centered::图3.2.1  创建FSI结构

完成分子建模后，可以支持多种结构文件类型下载，包括.pdb、.mol、.mol2、.xyz

.. figure:: image/example-PEO/下载结构文件.png
    :align: center
.. centered::图3.2.2  下载结构文件

下载FSI分子的pdb结构文件如下：

.. code-block:: 

    TITLE     bis(fluorosulfonyl)imide
    ATOM      1  NBT NS      1      -0.168  -0.820  -0.081  1.00  0.00
    ATOM      2  SBT NS      1      -1.632  -0.300  -0.128  1.00  0.00
    ATOM      3  SBT NS      1       0.960   0.020  -0.721  1.00  0.00
    ATOM      4  OBT NS      1       0.879   0.465  -2.120  1.00  0.00
    ATOM      5  OBT NS      1       1.575   0.970   0.179  1.00  0.00
    ATOM      6  OBT NS      1      -2.303  -0.173  -1.425  1.00  0.00
    ATOM      7  OBT NS      1      -2.428  -1.028   0.848  1.00  0.00
    ATOM      8  F1  NS      1       2.045  -1.173  -0.898  1.00  0.00
    ATOM      9  F1  NS      1      -1.649   1.230   0.367  1.00  0.00
    END

还建立了 :math:`\ce{Li^+}` 结构， :math:`\ce{Li^+}` 的pdb结构文件如下：

.. code-block:: 

    REMARK   1 File created by GaussView 6.0.16
    HETATM    1 Li           0      -1.265  -0.267   0.000                      Li
    END


此外，AuToFF可直接进行聚合物建模，建立PEO聚合物。如下：

.. figure:: image/example-PEO/创建PEO结构.png
    :align: center
.. centered::图3.2.3  创建PEO结构


聚合物PEO结构文件下载链接 :download:`PEO.pdb <files/PEO.pdb>`

   


选用适当力场和模拟软件
########################################################

选择适当的力场是进行MD模拟的基础，可以快速地获得准确的模拟结果。针对离子液体FSI选择OPLS力场即可，确定原子类型

.. figure:: image/example-PEO/根据力场选择原子类型.png
    :align: center
.. centered::图3.2.4  根据力场选择原子类型

.. note:: 

  * 点击结构视图中原子可进行配置原子类型

生成拓扑文件
########################################################

根据力场的选择即可生成拓扑文件的相关力场参数，包括LJ、键、键角、二面角参数，原子电荷。此外生成拓扑文件可支持多款计算软件，包括：GROMACS、LAMMPS、AMBER、Moltemplate、OpenMM、TINKER、CHARMM。下载的文件夹中除了力场拓扑文件之外还包含力场参数的文献来源。

.. figure:: image/example-PEO/生成拓扑文件.png
    :align: center
.. centered::图3.2.5  生成拓扑文件

.. note:: 

  * 点击下方显示标签按钮即可显示元素名称、原子ID、原子电荷。
  * 用户也可通过  **编辑**  按钮进行自行修改力场参数信息。

模拟体系建模
-------------------------------------------------------
构建体系
########################################################

首先，创建模拟体系。通过Packmol软件，我们将聚合物电解质体系的组成分子放入一个立方体的模拟盒子中。这个过程中立方体的盒子大小要略大于同等密度下聚合物电解质体系所需要的体积，以保证有足够的空间使得模拟体系内分子能够随机的分布并且模拟可以快速平衡。Packmol输入文件model.inp如下：

.. code-block:: 

  tolerance 2.0
  filetype pdb
  add_box_sides 1.5
  output model.pdb
    structure Li.pdb
      number 500
      inside cube 0. 0. 0. 150
    end structure
    structure FSI.pdb
      number 500
      inside cube 0. 0. 0. 150
    end structure
    structure PEO.pdb
      number 40
      inside cube 0. 0. 0. 150
    end structure

  

运行 **packmol < model.inp** 可生成model.pdb文件，该文件包含了锂离子聚合物电解质模拟体系中所有原子的坐标，但缺少键、键角等拓扑结构信息。将得到的model.pdb导入到VMD显示如下

.. figure:: image/example-PEO/packmol建立初始模型.bmp
    :align: center
.. centered::图3.2.6  模拟体系初始构型

构建拓扑文件
########################################################

拓扑文件是gromacs运行模拟所必需的文件，它提供了模拟体系中所有分子的拓扑结构、力场文件的引用、约束力参数……；拓扑文件必须包含三个层次：

* 参数级别；这一部分包括了力场设定
* 分子定义级别：这一部分包含了一个或多个分子对应的.itp文件。实际上，.itp 文件可以看做是 .top 文件分子定义级别（针对每单个分子）单独拿出储存的信息，他们形成了一个嵌套式的引用关系
* 体系级别：只包含体系的特定信息

锂离子聚合物电解质模拟体系的top文件model.top如下：

.. code-block:: 

  #define _FF_OPLS
  #define _FF_OPLSAA
  [ defaults ]
  1 3 yes 0.5 0.5
  #include "PEO_ATP.itp"
  #include "Li_ATP.itp"
  #include "FSI_ATP.itp"
  #include "PEO.itp"
  #include "Li.itp"
  #include "FSI.itp"
  [ system ]
  25DSPE+5AIE
  [ molecules ]
  PEO      40
  Li       500
  FSI      500


MD模拟
-------------------------------------------------------
能量最小化
########################################################

随后通过共轭梯度法优化初始结构，使得分子间的距离合适，没有较大的应力。gromacs能量最小化em.mdp输入如下：

.. code-block:: 
   
   define = -DFLEXIBLE
   integrator = cg
   nsteps = 10000
   emtol  = 100.0
   emstep = 0.01
   ;
   nstxout   = 100
   nstlog    = 50
   nstenergy = 50
   ;
   pbc = xyz
   cutoff-scheme            = Verlet
   coulombtype              = PME
   rcoulomb                 = 1.0
   vdwtype                  = Cut-off
   rvdw                     = 1.0
   DispCorr                 = EnerPres
   ;
   constraints              = none

MD平衡过程
########################################################

在模拟过程中，模拟步长设为２fs，采用Verlet算法来计算运动方程。模拟体系的三个方向均考虑周期性，是体相的模拟。为了使模拟体系快速合理达到平衡状态，采用梯度退火模拟。具体流程如下：等温等压系综下，模拟体系首先被缓慢加热到500 K，并在500 K下维持1 ns的NPT系综模拟，然后逐步将温度下降至400K ,并在400 K下维持1 ns的NPT系综模拟,最后再逐步将温度下降至目标温度298.15 K。当体系温度达到模拟的目标温度后，继续保持NPT系综计算2 ns，以保证模拟体系的能量、密度的性质趋于收敛，体系保持平衡。gromacs平衡过程eq.mdp输入如下：

.. code-block:: 
   
   define =
   integrator = md
   
   
   dt         = 0.002
   nsteps     = 5000000
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
   annealing_npoints = 5
   annealing_time = 0 1000 2000 3000 4000 5000 7000
   annealing_temp = 0 500 500 400 400 298.15 298.15
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

最后，在体系平衡的基础上，继续模拟2 ns ，并采样、分析、计算体系结构和性质等信息。gromacs模拟计算prod.mdp输入如下：

.. code-block:: 
      
   define =
   integrator = md
   
   
   dt         = 0.002
   nsteps     = 1000000
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
模拟平衡结构快照图
########################################################

取出模拟平衡后最后一帧结构，导入VMD即可查看快照图如下：

.. figure:: image/example-PEO/模拟平衡结构快照图.png
    :align: center
.. centered::图3.2.7  模拟平衡结构快照图

.. note:: 

  * gromacs转换成pdb结构文件命令： gmx  trjconv -f prod.xtc -s prod.tpr -o prod.pdb -dump 2000

径向分布函数（RDF）
########################################################


为了研究体系的局部结构特征，统计体系径向分布函数，计算 :math:`\ce{Li^+}` 的配位数，

.. math::
    & g_{𝛼𝛽}=\frac{\rho_{𝛼𝛽}(r)}{N_b/V} \\
    & n_{𝛼𝛽}=\rho_𝛽\int_{0}^{(r_{min})}g_{𝛼𝛽}(r)4𝜋r^2dr \\

其中，:math:`\ce{r_{min}}` 为径向分布函数中第一波谷对应的位置， :math:`{\rho_𝛽}` 为体系中平均粒子密度。


.. figure:: image/example-PEO/RDF.png
    :align: center
.. centered::图3.2.8  径向分布函数图

.. note:: 

  * gromacs可以生成径向分布函数，命令为：gmx rdf -f prod.xtc -s prod.tpr -o rdf.xvg -cn rdf_cn.xvg -bin 0.005 -b 1000 -e 2000 -rmax 1

均方位移(MSD)和扩散系数
########################################################

为了探究 :math:`\ce{Li^+}` 的扩散系数，gromacs可计算均方位移，模拟了不同温度下离子的扩散性质，如下图:

.. figure:: image/example-PEO/MSD.png
    :align: center
.. centered::图3.2.9  均方位移图

.. note:: 

  * gromacs可以计算均方位移，命令为：gmx msd -f eq.xtc -s eq.tpr  -beginfit 830 -endfit 1400  -trestart 0.002

继而可通过平衡分子动力学(EMD)模拟计算扩散系数，粒子的自扩散系数与其均方位移对时间的导数有关

.. math::
    D_s = \lim\limits_{\tau→∞}\frac{1}{6}\frac{d<(r_i(\tau)-r_i(0))^2>}{d\tau}

结果如下表：

.. table:: 不同温度下 :math:`\ce{Li^+}` 的扩散系数
   :widths: 30 70

   ==================== ======================================================================================================
   Temperature(K)        :math:`\ce{𝐷_( Li^+ ) (cm^2/s)}`
    298                   0.0001587 (+/- 1.382e-05) 1e-5
    303                   0.0001887 (+/- 3.181e-05) 1e-5
    313                   0.0002174 (+/- 1.996e-05) 1e-5 
    333                   0.0004883 (+/- 6.288e-05) 1e-5
   ==================== ======================================================================================================
