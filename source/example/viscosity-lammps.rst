.. _viscosity-lammps:

黏度计算-AuToFF生成LAMMPS的力场拓扑data文件
================================================

黏度是影响电解液离子电导率的重要影响因素之一。黏度是电解液重要的基础物性，与电解液和电池的性能高度相关。能斯特–爱因斯坦方程揭示了黏度与粒子扩散的关联，表明黏度直接影响电解液中粒子的输运性质，从而影响电池的极化和倍率性能。此外，黏度会影响电解液对其他电池材料的润湿性，这也是在实际生产过程中必须考量的因素。下面利用 **Packmol** 建模获得原子坐标，继而使用 **AuToFF** 生成力场参数、电荷参数，再者 **Moltemplate** 补全拓扑信息、力场信息，并生成 **LAMMPS** 对应形式的力场拓扑data格式文件,最后采用LAMMPS进行非平衡态分子动力学方法(NEMD)方法模拟黏度性质。


力场辅助工具-AuToFF
-------------------------------------------------------

确定起始构型
########################################################

确定电解液溶剂EC分子结构，可选择点击AuToFF程序-全原子模块进行2D建模（如图1），点击“ **生成3d结构视图** ”按钮即可3D显示。

.. figure:: image/viscosity-lammps/创建EC分子结构.png
    :align: center
.. centered:: 图5.1.1  创建EC分子结构

同上，依次建立EMC分子、 :math:`\ce{Li^+}`离子 和 :math:`\ce{{PF_6}^-}` 阴离子。此外 :math:`\ce{Li^+}` 和 :math:`\ce{{PF_6}^-}` 离子需在离子液体模块建立。


选用适当力场和模拟软件
########################################################

选择适当的力场是进行MD模拟的基础，可以快速地获得准确的模拟结果。针对溶剂分子EC和EMC选择GAFF2力场，而 :math:`\ce{Li^+}` 离子选择CL&P力场，离子液体 :math:`\ce{{PF_6}^-}` 选择OPLS-2009IL/0.8力场即可，确定原子类型

.. figure:: image/viscosity-lammps/根据力场选择原子类型.png
    :align: center
.. centered:: 图5.1.2  根据力场选择原子类型

.. note:: 

  * 点击结构视图中原子可进行配置原子类型
  * 选择电荷类型：支持多种电荷计算方法，此处选择XTB-RESP，可以快速得到RESP近似电荷

生成拓扑文件
########################################################

根据力场的选择即可生成拓扑文件的相关力场参数，包括LJ、键、键角、二面角参数、原子电荷，此外生成拓扑文件可支持多款计算软件。接下来，我们将结合packmol和moltemplate来实现复杂体系建模，并生成lammps计算所需data力场拓扑文件。因此，选择moltemplate生成该程序的输入文件。

.. figure:: image/viscosity-lammps/生成拓扑文件.png
    :align: center
.. centered:: 图5.1.3  生成拓扑文件

.. note:: 

  * 点击下方显示标签按钮即可显示元素名称、原子ID、原子电荷。
  * 用户也可通过 **编辑** 按钮进行自行修改原子电荷、力场参数信息。

由于 :math:`\ce{{PF_6}^-}` 选择OPLS-2009IL/0.8力场，进行电荷缩放，为保证体系呈电中性， :math:`\ce{Li^+}` 离子电荷修改为0.8。

.. figure:: image/viscosity-lammps/锂离子电荷修改.png
    :align: center
.. centered:: 图5.1.4  锂离子电荷修改

一键下载的moltemplate压缩包中包含 **pdb结构文件，packmol的单组份input文件，moltemplate的lt输入文件**

模拟体系建模
-------------------------------------------------------
构建体系
########################################################

首先，创建模拟体系。通过Packmol软件，我们将电解液组成分子放入一个立方体的模拟盒子中。这个过程中立方体的盒子大小要略大于同等密度下离子液体所需要的体积，以保证有足够的空间使得离子液体分子能够随机的分布并且模拟可以快速平衡。将AuToFF创建并下载好每个组分的拓扑文件，然后把pdb文件拷贝到packmol文件夹，调用packmol程序生成模拟的盒子。Packmol输入文件model.inp如下：

.. code-block:: 

 tolerance 2.0
 filetype pdb
 add_box_sides 1.5
 output model.pdb
   structure EC.pdb
       number 157
       inside cube 0. 0. 0. 50
   end structure
   structure EMC.pdb
       number 309
       inside cube 0. 0. 0. 50
   end structure
   structure Li.pdb
     number 45
       inside cube 0. 0. 0. 50
   end structure
   structure PF6.pdb
       number 45
       inside cube 0. 0. 0. 50
   end structure

 

运行 **packmol < model.inp** 可生成model.pdb文件，该文件包含了电解液模拟体系中所有原子的坐标，但缺少键、键角等拓扑结构信息。将得到的model.pdb可导入到VMD显示查看


构建力场拓扑文件
########################################################

力场拓扑文件是运行MD模拟所必需的文件，接下来将利用packmol生成的体系原子坐标文件，结合moltemplate补全拓扑信息、力场信息等，并生成lammps的data格式文件。其中AuToFF生成了电解液模拟体系中各个组分的moltemplate输入文件，下载链接 :download:`EC.lt <files/EC.lt>` :download:`EMC.lt <files/EMC.lt>` :download:`Li.lt <files/Li.lt>` :download:`PF6.lt <files/PF6.lt>`

moltemplate输入文件system.lt如下：

.. code-block:: 

   import "EC.lt"
   import "EMC.lt"
   import "Li.lt"
   import "PF6.lt"
   
   ec = new EC   [157]
   emc = new EMC   [309]
   li = new Li   [45]
   pf6 = new PF6   [45]
   
   write_once("Data Boundary") {
         0.00000   50.00000  xlo xhi
         0.00000   50.00000  ylo yhi
         0.00000   50.00000  zlo zhi

.. note:: 

    * moltemplate输入文件system.lt中各个组分顺序需与packmol输入文件model.inp组分顺序保持一致。

运行 **moltemplate.sh -pdb model.pdb system.lt** 即可生成 :download:`system.data <files/system.data>` 拓扑信息文件和system.in.settings :download:`system.in.settings <files/system.in.settings>` 力场信息文件，该文件可在lammps中直接使用。

MD模拟
-------------------------------------------------------

借助分子动力学软件LAMMPS，利用非平衡态分子动力学方法计算体系黏度，即在非平衡态下对体系施加剪切，计算不同剪切速率下的稳态粘度，外推至零剪切速率下得到零切粘度。LAMMPS计算参数in.msd输入文件如下：

.. code-block:: 
   
  #-------------------------------------------------------------------------------------------------------------------#

  variable        temperature     equal   298
  variable        timestep        equal   1
  variable        Tdamp           equal   ${timestep}*100
  variable        Pdamp           equal   ${timestep}*1000
  variable        drag            equal   0.7
  variable        tequ            equal   1000
  variable        trun            equal   1000000
  variable        srate           equal   0.003
  variable        scaling         equal   1e6/1e15

  #-------------------------------------------------------------------------------------------------------------------#
  
  units           real
  boundary        p p p
  atom_style      full
  pair_style      lj/cut/coul/long 15.0
  pair_modify     mix arithmetic
  special_bonds   lj 0.0 0.0 0.5 coul 0.0 0.0 0.8333
  kspace_style    pppm 1.0e-5
  bond_style      harmonic
  angle_style     harmonic
  dihedral_style  fourier
  improper_style  cvff
  read_data       system.data     
  include         system.in.settings
  thermo          1000
  timestep        ${timestep}
  
  #-------------------------------------------------------------------------------------------------------------------#
  
  #minimize       1.0e-4 1.0e-6 100 1000
  minimize         0.0 1.0e-8 1000 100000
  fix             1 all nve/limit 0.1
  fix             2 all langevin ${temperature} ${temperature} ${Tdamp} 123456 zero yes
  run             1000
  unfix           2
  unfix           1
  
  #-------------------------------------------------------------------------------------------------------------------#
  
  fix             npt all npt temp ${temperature} ${temperature} ${Tdamp} iso 0 0 ${Pdamp} drag ${drag}
  run             ${tequ}
  unfix           npt
  
  write_data      data.final
  
  reset_timestep  0
  
  #-------------------------------------------------------------------------------------------------------------------#
  
  change_box      all triclinic
  
  kspace_style    pppm 1.0e-5
  
  fix             deform all deform 1 xy erate ${srate} remap v
  
  fix             sllod all nvt/sllod temp ${temperature} ${temperature} ${Tdamp}
  
  compute         usual all temp
  
  compute         tilt all temp/deform
  
  thermo_style    custom step temp c_usual epair etotal press pxy
  
  thermo_modify   temp tilt
  
  #--------------------------------------------------------------------------------------------------------#
  
  fix             rescale all temp/rescale 1 ${temperature} ${temperature} 1.0 1.0
  
  run             10000
  unfix           rescale
  
  run             10000
  reset_timestep  0
  
  #--------------------------------------------------------------------------------------------------------#
  
  variable        visc equal -pxy/(${srate})*${scaling}
  
  fix             vave all ave/time 10 100 1000 v_visc ave running start 500000
  
  thermo_style    custom step temp press pxy v_visc f_vave
  
  thermo_modify   temp tilt
  
  run             ${trun}



MD结果分析
-------------------------------------------------------
黏度分析
########################################################

分析LAMMPS输出文件log.lammps得到黏度性质，其中LAMMPS输入文件已进行结果预处理，输出f_vave数值已进行平均处理，直接读取最后一步的数据即可，即为稳态黏度数值。此外，还模拟了不同温度下的稳态黏度，结果展示如下图：


.. figure:: image/viscosity-lammps/黏度结果.png
    :align: center
.. centered:: 图5.1.5  不同温度和剪切速率下的体系稳态粘度





