分子力场类型
************************************

分子力场根据量子力学的波恩-奥本海默近似，一个分子的能量可以近似看作构成分子的各个原子的空间坐标的函数，简单地讲就是分子的能量随分子构型的变化而变化，而描述这种分子能量和分子结构之间关系的就是分子力场函数。一般而言，分子力场函数由以下几个部分构成：
 
 * 键伸缩能：构成分子的各个化学键在键轴方向上的伸缩运动所引起的能量变化
 * 键角弯曲能：键角变化引起的分子能量变化
 * 二面角扭曲能：单键旋转引起分子骨架扭曲所产生的能量变化
 * 非键相互作用：包括范德华力、静电相互作用等与能量有关的非键相互作用
 * 交叉能量项：上述作用之间耦合引起的能量变化

 不同的分子力场会选取不同的函数形式来描述上述能量与体系构型之间的关系。

GAFF:
======

.. math::
    E_{pair} = & \sum_{bonds} K_r(r-r_{eq})^2 + \sum_{amgles} K_{\theta}(\theta -\theta_{eq})^2 + \sum_{dihedrals} \frac{V_n}{2} [1+cos(n\phi-\gamma)] + \sum_{i<j} [\frac{A_{ij}}{R_{ij}^{12}} \\
    & - \frac{B_{ij}}{R_{ij}^6} + \frac{q_{i}q_{j}}{\varepsilon R_{ij}}]


AMBER:
========

.. math::
    E_{pot} = & \sum_{b} K_2(b-b_0)^2 + \sum_{\theta}H_{\theta}(\theta-\theta_0)^2 + \sum_{\phi} \frac{V_n}{2}[1 + \cos(n\phi-\phi_0)] \\
    & + \sum \varepsilon[(r^*/r)^{12}-2(r^*/r)^6] + \sum \frac{q_{i}q_{j}}{\varepsilon_{ij}r_{ij}} + \sum[\frac{C_{ij}}{r_{ij}^{12}}] + \sum[\frac{C_{ij}}{r_{ij}^{12}} - \frac{D_{ij}}{r_{ij}^{10}}]


CVFF:
========

.. math::
    V = & \sum {D_b[1-e^{-\alpha(b-b_0)}]^2 - D_b} + \frac{1}{2}\sum H_0(\theta-\theta_0)^2 + \frac{1}{2}\sum H_{\phi}(1+s\cos{n\phi}) + \frac{1}{2}\sum H_{\chi}\chi^2 \\
    & + \sum \sum F_{bb'}(b-b_0)(b'-b_0') + \sum \sum F_{\theta \theta'}(\theta - \theta_0)(\theta' - \theta_0') + \sum \sum F_{b^{\theta}}(b-b_0)(\theta - \theta_0) \\
    & + \sum F_{\phi^{\theta \theta'}} \cos \phi(\theta-\theta_0)(\theta'-\theta_0') + \sum \sum F_{\chi \chi'}\chi \chi' + \sum \frac{A}{r^{12}} - \frac{B}{r^6} + \sum \frac{q_{i}q_{j}}{r}

Bond:

1) Morse:

.. math::
    E = D*(1-exp(-ALPHA*(R-R0)))^2

2) Harmonic:

.. math::
    E = K2*(R-R0)^2

PCFF:
========

.. math::
    & E = \sum E^b + \sum E^{\alpha} + \sum E^0 + \sum E^t + \sum E^{bb} + \sum E^{ab} + \sum E^{aa} + \sum E^{at} + \sum E^{bt} + \sum E^{elec} + \sum E^{VDW} \\
    & E^b = \sum_{i=2}^4 k_i^b (b-b_0)^i \\
    & E^a = \sum_{i=2}^4 k_i^a (\theta-\theta_0)^i \\
    & E^t = \sum_{i=1}^4 k_i^t (1-\cos i\theta) \\
    & E^0 = k^0 (\chi -\chi_0)^2 \\
    & {E^{bb}, E^{aa}, E^{ab}} = k^c (s-s_0)(s'-s'_0) \\
    & {E^{bt}} = (b-b_0)\sum_{i=1}^3 k_i^c (1-\cos i\phi) \\
    & {E^{at}} = (\theta-\theta_0)\sum_{i=1}^3 k_i^c (1-\cos i\phi) \\
    & E^{elec} = \sum_{ij} \frac{q_{i}q_{j}}{r_{ij}} \\
    & E^{VDW} = \sum_{ij} \epsilon_{ij} \left[ 2(\frac{r_{ij}^0}{r_{ij})^9 - 3(\frac{r_{ij}^0}{r_{ij})^6 \right]

CFF:
=========
.. math::
    E_{total} = & \sum_b [k_2(b-b_0)^2 + k_3(b-b_0)^3 + k_4(b-b_0)^4] + \sum_0 [k_2(\theta-theta_0)^2 + k_3(\theta-theta_0)^3 + k_4(\theta-theta_0)^4] \\
                & +\sum_{\phi} [k_1(1-\cos \phi) + k_2(1-\cos2\phi) + k_3(1-\cos 3\phi)] + \sum_{\chi} k_2\chi^2 + \sum_{b,b'} k(b-b_0)(b'-b'_0) \\
                & +\sum_{b,\theta} k(b-b_0)(\theta-\theta_0) + \sum_{b,\phi} (b-b_0)[k_1\cos \phi + k_2\cos 2\phi + k_3\cos 3\phi] \\
                & +\sum_{\theta,\phi} k(\theta-\theta_0)[k_1\cos \phi + k_2\cos 2\phi + k_3\cos 3\phi] + \sum_{b,\theta} k(\theta'-theta_0')(\theta-theta_0) \\
                & + \sum_{\theta,\theta,\phi} k(\theta-theta_0)(\theta'-theta_0')\cos\phi + \sum_{i,j} frac{q_iq_j}{r_{ij}} \\
                & +\sum_{i,j} \epsilon_{ij}[2(\frac{r_{ij}^0}{r_{ij}})^9 - 3(\frac{r_{ij}^0}{r_{ij}})^6] 

OPLS:
===========
.. math::
    & E_{bond} = \sum_i k_{b,i}(r_i - r_{0,i})^2 \\
    & E_{bend} = \sum_i k_{\vartheta,i}(\vartheta_i - \vartheta_{0,i})^2 \\
    & E_{torsion} = \sum_i \{V_{1,i}(1 + \cos\phi_i)/2 + V_{2,i}(1 + \cos2\phi_i)/2 + V_{3,i}(1 + \cos3\phi_i)/2\} \\
    & E_{nb} = \sum_{i<j} \{q_iq_je^2/r_{ij} + 4\epsilon_{ij}[(\sigma_{ij}/r_{ij})^{12} - (\sigma_{ij}/r_{ij})^6]\} \\
    & \sigma_{ij} = \sqrt{\sigma_{ii}\sigma_{jj}} \\
    & \epsilon_{ij} = \sqrt{\epsilon_{ii}\epsilon_{jj}}

MMFF:
=========
.. math::
    V_{total} = & \sum_{bonds} K_{bond}(r-r_{eq})^2(1+cs(r-r{eq}) + frac{2}{7}(cs^2(r-r_{eq})^2)) \\
                & + \sum_{angle} K_{\theta}(\theta-\theta_{eq})^2(1+cb(\theta-\theta_{eq})) + \sum_{angle,linear} K_{al}(1+\cos(\theta)) \\
                & + \sum_{stretch,bend} (K_{ijk}(r_{ij}-r_{eq}) + K_{kji}(r_{kj}-r_{eq}))(\theta-\theta_{eq}) + \sum_{outofplane} K_{OOP}(\chi)^2 \\
                & + \sum_{dihedrals} \frac{V_1}{2}[1+\cos(\phi)] + \frac{V_2}{2}[1+\cos(2\phi)] + \frac{V_3}{2}[1+\cos(3\phi)] \\
                & + \sum_{i<j} [\epsilon(\frac{1.07\sigma}{r_{ij}+0.07\sigma})^7 (\frac{1.12\sigma^7}{r_{ij}^7+0.07\sigma^7}-2) - \frac{q_iq_j}{D(r_{ij}+\delta)}]

