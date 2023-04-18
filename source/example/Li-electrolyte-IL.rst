.. _Li-electrolyte-IL:

锂离子电池离子液体电解质体系的MD模拟
================================================

锂电池因其较高能量密度受到学术界和工业界的广泛关注。电解液作为锂电池的一个关键组成，对锂电池性能具有十分重要的影响。离子液体是由有机阳离子以及有机或无机阴离子所组成的室温有机熔融盐，具有低的挥发性、不可燃、高的热以及化学稳定性、宽的电化学窗口、高的离子电导率以及结构可调谐等优势，得到了广泛的研究与应用。

力场辅助工具-AuToFF
-------------------------------------------------------

确定起始构型
########################################################
确定双(氟磺酰)亚胺(FSI)阴离子结构，可选择点击AuToFF程序-离子液体模块进行2D建模（如图1），点击“ **生成3d结构视图** ”按钮即可3D显示。

.. figure:: image/example-IL/创建FSI结构.png
    :align: center
.. centered:: 图3.1.1  创建FSI分子结构

完成分子建模后，可以支持多种结构文件类型下载，包括.pdb、.mol、.mol2、.xyz

.. figure:: image/example-IL/下载结构文件.png
    :align: center
.. centered:: 图3.1.2  下载结构文件

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

同理，建立了THF分子结构如下：

.. figure:: image/example-IL/创建THF分子结构.png
    :align: center
.. centered::图3.1.3  创建THF分子结构

THF分子的pdb结构文件如下：

.. code-block:: 

    ATOM      1  O1  THF     1       1.138  -0.023  -0.012  1.00  0.00           O
    ATOM      2  C2  THF     1       0.289  -1.168   0.135  1.00  0.00           C
    ATOM      3  C3  THF     1      -1.119  -0.718  -0.192  1.00  0.00           C
    ATOM      4  C4  THF     1      -1.096   0.731   0.223  1.00  0.00           C
    ATOM      5  C5  THF     1       0.316   1.147  -0.124  1.00  0.00           C
    ATOM      6  H6  THF     1       0.365  -1.509   1.173  1.00  0.00           H
    ATOM      7  H7  THF     1       0.635  -1.970  -0.522  1.00  0.00           H
    ATOM      8  H8  THF     1      -1.883  -1.309   0.317  1.00  0.00           H
    ATOM      9  H9  THF     1      -1.292  -0.791  -1.273  1.00  0.00           H
    ATOM     10  H10 THF     1      -1.254   0.808   1.305  1.00  0.00           H
    ATOM     11  H11 THF     1      -1.854   1.341  -0.275  1.00  0.00           H
    ATOM     12  H12 THF     1       0.696   1.928   0.541  1.00  0.00           H
    ATOM     13  H13 THF     1       0.383   1.506  -1.157  1.00  0.00           H
    END
 
同理，建立了TTE分子结构如下：

.. figure:: image/example-IL/创建TTE分子结构.png
    :align: center
.. centered::图3.1.4  创建TTE分子结构

TTE分子的pdb结构文件如下：

.. code-block:: 

    ATOM      1  C1  TTE     1       1.142   0.720  -2.222  1.00  0.00           C
    ATOM      2  C2  TTE     1       0.567  -0.382  -1.346  1.00  0.00           C
    ATOM      3  C3  TTE     1       1.027  -1.774  -1.781  1.00  0.00           C
    ATOM      4  O4  TTE     1       2.454  -1.899  -1.714  1.00  0.00           O
    ATOM      5  C5  TTE     1       2.882  -3.210  -2.100  1.00  0.00           C
    ATOM      6  C6  TTE     1       4.403  -3.383  -2.012  1.00  0.00           C
    ATOM      7  F7  TTE     1       4.779  -4.634  -2.394  1.00  0.00           F
    ATOM      8  F8  TTE     1       0.935  -0.190  -0.047  1.00  0.00           F
    ATOM      9  F9  TTE     1      -0.795  -0.353  -1.362  1.00  0.00           F
    ATOM     10  F10 TTE     1       0.773   0.545  -3.518  1.00  0.00           F
    ATOM     11  F11 TTE     1       0.660   1.937  -1.845  1.00  0.00           F
    ATOM     12  F12 TTE     1       2.261  -4.100  -1.283  1.00  0.00           F
    ATOM     13  F13 TTE     1       2.429  -3.437  -3.358  1.00  0.00           F
    ATOM     14  F14 TTE     1       5.042  -2.519  -2.846  1.00  0.00           F
    ATOM     15  H15 TTE     1       2.232   0.764  -2.170  1.00  0.00           H
    ATOM     16  H16 TTE     1       0.683  -1.969  -2.803  1.00  0.00           H
    ATOM     17  H17 TTE     1       0.564  -2.514  -1.120  1.00  0.00           H
    ATOM     18  H18 TTE     1       4.767  -3.208  -0.996  1.00  0.00           H
    END
 
还建立了 :math:`\ce{Li^+}` 结构， :math:`\ce{Li^+}` 的pdb结构文件如下：

.. code-block:: 

    REMARK   1 File created by GaussView 6.0.16
    HETATM    1 Li           0      -1.265  -0.267   0.000                      Li
    END

  


选用适当力场和模拟软件
########################################################

选择适当的力场是进行MD模拟的基础，可以快速地获得准确的模拟结果。针对离子液体FSI选择OPLS力场即可，确定原子类型

.. figure:: image/example-IL/根据力场选择原子类型.png
    :align: center
.. centered::图3.1.5  根据力场选择原子类型

.. note:: 

  * 点击结构视图中原子可进行配置原子类型

生成拓扑文件
########################################################

根据力场的选择即可生成拓扑文件的相关力场参数，包括LJ、键、键角、二面角参数，原子电荷。此外生成拓扑文件可支持多款计算软件，包括：GROMACS、LAMMPS、AMBER、Moltemplate、OpenMM、TINKER、CHARMM。下载的文件夹中除了力场拓扑文件之外还包含力场参数的文献来源。

.. figure:: image/example-IL/生成拓扑文件.png
    :align: center
.. centered::图3.1.6  生成拓扑文件

.. note:: 

  * 点击下方显示标签按钮即可显示元素名称、原子ID、原子电荷。
  * 用户也可通过 **编辑** 按钮进行自行修改力场参数信息。

模拟体系建模
-------------------------------------------------------
构建体系
########################################################

首先，创建模拟体系。通过Packmol软件，我们将离子液体的组成分子放入一个立方体的模拟盒子中。这个过程中立方体的盒子大小要略大于同等密度下离子液体所需要的体积，以保证有足够的空间使得离子液体分子能够随机的分布并且模拟可以快速平衡。将AuToFF创建并下载好每个组分的拓扑文件，然后把pdb文件拷贝到packmol文件夹，调用packmol程序生成模拟的盒子。Packmol输入文件model.inp如下：

.. code-block:: 

  tolerance 2.0
  filetype pdb
  add_box_sides 1.5
  output model.pdb
    structure Li.pdb
      number 63
        inside cube 0. 0. 0. 60
    end structure
    structure FSI.pdb
        number 63
        inside cube 0. 0. 0. 60
    end structure
    structure THF.pdb
        number 310
        inside cube 0. 0. 0. 60
    end structure
    structure TTE.pdb
        number 165
        inside cube 0. 0. 0. 60
    end structure

 

运行 **packmol < model.inp** 可生成model.pdb文件，该文件包含了锂离子离子液体电解质模拟体系中所有原子的坐标，但缺少键、键角等拓扑结构信息。将得到的model.pdb导入到VMD显示如下

.. figure:: image/example-IL/packmol建立初始模型.bmp
    :align: center
.. centered::图3.1.7  模拟体系初始构型

构建拓扑文件
########################################################

拓扑文件是gromacs运行模拟所必需的文件，它提供了模拟体系中所有分子的拓扑结构、力场文件的引用、约束力参数……；拓扑文件必须包含三个层次：

* 参数级别；这一部分包括了力场设定
* 分子定义级别：这一部分包含了一个或多个分子对应的.itp文件。实际上，.itp 文件可以看做是 .top 文件分子定义级别（针对每单个分子）单独拿出储存的信息，他们形成了一个嵌套式的引用关系
* 体系级别：只包含体系的特定信息

锂离子离子液体电解质模拟体系的top文件model.top如下：

.. code-block:: 

  #define _FF_OPLS
  #define _FF_OPLSAA
  [ defaults ]
  1 3 yes 0.5 0.5
  #include "Li_ATP.itp"
  #include "FSI_ATP.itp"
  #include "THF_ATP.itp"
  #include "TTE_ATP.itp"
  #include "Li.itp"
  #include "FSI.itp"
  #include "THF.itp"
  #include "TTE.itp"
  [ system ]
  63Li+63FSI+310THF+165TTE
  [ molecules ]
  Li      63
  FSI      63
  THF      310
  TTE      165


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

MD采样过程
########################################################
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

.. figure:: image/example-IL/模拟平衡结构快照图.bmp
    :align: center
.. centered::图3.1.8  模拟平衡结构快照图

.. note:: 

  * gromacs转换成pdb结构文件命令： gmx  trjconv -f prod.xtc -s prod.tpr -o prod.pdb -dump 2000

径向分布函数（RDF）
########################################################


为了研究体系的局部结构特征，统计体系径向分布函数，计算 :math:`\ce{Li^+}` 的配位数，

.. math::
    & g_{𝛼𝛽}=\frac{\rho_{𝛼𝛽}(r)}{N_b/V} \\
    & n_{𝛼𝛽}=\rho_𝛽\int_{0}^{(r_{min})}g_{𝛼𝛽}(r)4𝜋r^2dr \\

其中，:math:`\ce{r_{min}}` 为径向分布函数中第一波谷对应的位置， :math:`{\rho_𝛽}` 为体系中平均粒子密度。


.. figure:: image/example-IL/RDF.png
    :align: center
.. centered:: 图3.1.9  径向分布函数图

.. note:: 

  * gromacs可以生成径向分布函数，命令为：gmx rdf -f prod.xtc -s prod.tpr -o rdf.xvg -cn rdf_cn.xvg -bin 0.005 -b 1000 -e 2000 -rmax 1

均方位移(MSD)和扩散系数
########################################################

为了探究 :math:`\ce{Li^+}` 的扩散系数，gromacs可计算均方位移，模拟了不同温度下离子的扩散性质，如下图:

.. figure:: image/example-IL/MSD.png
    :align: center
.. centered:: 图3.1.10  均方位移图

.. note:: 

  * gromacs可以计算均方位移，命令为：gmx msd -f eq.xtc -s eq.tpr  -beginfit 830 -endfit 1400  -trestart 0.002

继而可通过平衡分子动力学(EMD)模拟计算扩散系数，粒子的自扩散系数与其均方位移对时间的导数有关

.. math::
    D_s = \lim\limits_{\tau→∞}\frac{1}{6}\frac{d<(r_i(\tau)-r_i(0))^2>}{d\tau}

计算所得，298K温度下 :math:`\ce{Li^+}` 的扩散系数为0.0812 (+/- 0.0113) 1e-5 :math:`\ce{(cm^2/s)}`


