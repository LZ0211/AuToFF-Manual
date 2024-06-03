AuToFF软件简介
================================================   


AuToFF (Auxiliary Tools of Force Field) 是一个完全自主开发的可视化力场辅助工具。AuToFF能实现2D分子建模、模型三维显示、支持现有的所有主流力场和二流力场形式、支持基于机器学习的第一性原理精度的RESP原子电荷、支持生成多款分子动力学程序或建模工具的输入格式（包括gromacs、lammps、amber、moltemplate、openmm、tinker、charmm、raspa、NAMD、GOMC、Gaussian、ORCA、Gear）、支持多种碳材料/聚合物的建模、支持周期性晶体结构、MOFs/COFs材料的力场拓扑文件生成、后期还将涵盖生物大分子建模以及残基库的设计搭建。该工具解决了现有的力场拓扑文件生成工具的诸多局限性，如：无法适应所有的力场形式，缺乏复杂体系建模功能，不能正确分配体系的力场参数以及难以计算分配体系中的原子电荷等。因此，AuToFF工具可以极大的降低分子模拟用户使用门槛，进而使得分子动力学模拟在生物，药学，化学，材料科学的研究中发挥着越来越重要的作用。  

AuToFF工具根据不同的力场适用条件和特定研究对象分成以下模块，分别为

 * 全原子力场
 * 联合原子力场
 * 离子液体
 * 碳材料
 * 金属/共轭有机框架
 * 无机晶体
 * 聚合物建模
 * 多体势
 * 反应力场
 * 粗粒化
 * 生物大分子（暂未开放）
 * QM/MM建模（暂未开放）

已上线功能如下：

 * 实现2D分子建模及其3D分子显示
 * 支持复杂体系建模，包括碳材料(富勒烯、石墨烯、碳纳米管)和聚合物体系(均聚物、嵌段共聚物、无规共聚物)
 * 提供常用分子结构库、晶体材料结构库和金属/共轭有机框架结构库
 * 提供类似ChemDraw的二维聚合物建模，可自由定义聚合物单体结构
 * 支持现有的主流力场（GAFF、CVFF、OPLS、CGenFF）、二代力场(CFF、COMPASS)、通用力场形式(UFF、Dreiding)、联合原子力场（TraPPE-UA、OPLS-UA、GROMOS）、13种水模型力场（OPC, OPC3, SPC, SPC/E, SPC/Eb, TIP3P, TIPS3P, TIP3P-FB, TIP4P, TIP4P/Ew, TIP4P/2005, TIP4P/ICE, TIP4P/ε ）、4种重水模型
 * 针对特定体系支持使用力场，如离子液体体系（CL&P、OPLS-2009IL、Merz离子力场、Haiyang Zhang离子力场)，界面体系（IFF力场），MOF体系（UFF4MOF、UFF4MOFII、ZIF-FF）、黏土体系（ClayFF）
 * 支持多种类型原子电荷的计算，包括基于机器学习的第一性原理精度的RESP原子电荷、MMFF94、AM1-BCC、Qeq、RESP、CM5、1.2*CM5、1.14*CM1A和1.14*CM1A-LBCC
 * 针对MOF/COF等周期性体系支持多种电荷计算方法，包括GMP-QEq、MEPO-QEq、m-CBAC、EQEq、I-QEq
 * 高精度的机器学习预测MOF和COF材料的REPEAT和DDEC电荷
 * 通过选择杂化力场进行单个力场参数缺失的补齐
 * 支持多种结构数据文件导入和分子结构文件下载
 * 支持生成简单分子体系、聚合物体系、金属/共轭有机框架体系、晶体材料体系、界面体系的力场拓扑文件
 * 支持生成多款分子动力学或蒙特卡洛计算软件的力场拓扑文件或建模工具的输入文件,包括LAMMPS, GROMACS, AMBER, Moltemplate, OpenMM, Tinker, CHARMM，NAMD，RASPA和GOMC软件
 * 支持Gaussian的ONIOM，ORCA和BDF软件的QM/MM输入参数生成
 * 支持mSeminario方法拟合力常数，支持Gaussian、ORCA、BDF和XTB四种软件的Hessian文件
 * 支持非混合规则的原子对LJ参数检索
 * ReaxFF反应力场和多体势的数据库
 * 全原子—Martini3 粗粒化力场映射
 * 无定形盒子的建模功能（开发完，前端未上线）
 
免费使用AuToFF
================================================ 
AuToFF软件是一款免费在线软件，有意使用AuToFF软件可直接访问 https://cloud.hzwtech.com/web/product-service?id=36


引用说明
================================================ 

**使用AuToFF工具进行后续相关计算，请引用下面的参考文献**

Wang, C.; Li, W.; Liao, K.; Wang, Z.; Wang, Y.; Gong, K. AuToFF Program, Vesrion 1.0. Hzwtech. Shanghai 2023, see https://cloud.hzwtech.com/web/product-service?id=36.