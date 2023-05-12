.. _ICE-melt:

冰块融化的MD模拟及TIP4P/ICE水模型使用
================================================

目前存在很多种水分子模型用于预测液态水的物理特性，或确定液态水的（未知）结构。AuToFF支持多种水模型，包括OPC, OPC3, SPC, SPC/E, SPC/Eb, TIP3P, TIPS3P, TIP3P-FB, TIP4P, TIP4P/Ew, TIP4P/2005, TIP4P/ICE, TIP4P/ε。其中，TIP4P/ICE是专门用来描述冰的一系列性质的水模型，对水的相图有较好的还原（1bar下冰-Ih熔点为272.2K）。TIP4P/ICE是一种刚性平面的四点水模型，亦即除了一个O和2个H原子外还加入了一个虚原子（在氧附近），虚原子仅具有负电荷，改善了水分子周围的静电分布。

.. figure:: image/example-ICE/TIP4P.bmp
    :align: center
.. centered:: 图3.3.1  TIP4P构型

接下来将介绍使用TIP4P/ICE水模型进行冰块融化的分子动力学模拟细节。



力场辅助工具-AuToFF
-------------------------------------------------------

确定起始构型
########################################################

选择AuToFF-全原子力场模块，上传冰晶体cif结构文件，水模型力场选择TIP4P/ICE，即可自动转成四点水模型的结构。
冰晶体的单胞结构文件下载链接 :download:`H2O-Ice-IV.cif <files/H2O-Ice-IV.cif>`

.. figure:: image/example-ICE/根据力场选择原子类型.bmp
    :align: center
.. centered:: 图3.3.2  确定冰晶体结构

选择力场
########################################################

TIP4P/ICE是专门用来描述冰的一系列性质的水模型，对水的相图有较好的还原（1bar下冰-Ih熔点为272.2K），因此模拟冰融化过程也采用TIP4P/ICE力场模型。

.. figure:: image/example-ICE/根据力场选择原子类型.bmp
    :align: center
.. centered:: 图3.3.3  根据力场选择原子类型

生成拓扑文件
########################################################

根据力场的选择即可生成拓扑文件的相关力场参数，包括LJ、键、键角参数，原子电荷。此外生成拓扑文件可支持多款计算软件，包括：GROMACS、LAMMPS、AMBER、Moltemplate、OpenMM、TINKER、CHARMM。下载的文件夹中除了力场拓扑文件之外还包含力场参数的文献来源。针对晶体结构支持扩胞操作，此处设置的是5*5*5

.. figure:: image/example-ICE/生成拓扑结构.bmp
    :align: center
.. centered:: 图3.3.4  生成冰晶体拓扑结构

冰晶体的结构文件下载链接 :download:`ICE.pdb <files/ICE.pdb>`

冰晶体模拟体系的top文件下载链接 :download:`ICE.top <files/ICE.top>`

MD模拟
-------------------------------------------------------

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
      annealing = single
      annealing_npoints = 2
      annealing_time = 0 2000 ;ps
      annealing_temp = 0 350
      ;
      pbc = xyz
      cutoff-scheme = Verlet
      coulombtype   = PME
      rcoulomb      = 0.9
      vdwtype       = cut-off
      rvdw          = 0.9
      DispCorr      = EnerPres
      ;
      Tcoupl  = V-rescale
      tau_t   = 0.2
      tc_grps = system
      ref_t   = 298.15
      ;
      Pcoupl     = Berendsen
      pcoupltype = isotropic
      tau_p = 0.5
      ref_p = 1.01325
      compressibility = 4.5e-5
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

冰融化的轨迹变化通过VMD作图如下：

.. figure:: image/example-ICE/ICE-melt.gif
    :align: center
.. centered:: 图3.3.5  冰融化过程

