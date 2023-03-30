.. _forcefields:

分子力场类型
================================================

分子力场根据量子力学的波恩-奥本海默近似，一个分子的能量可以近似看作构成分子的各个原子的空间坐标的函数，简单地讲就是分子的能量随分子构型的变化而变化，而描述这种分子能量和分子结构之间关系的就是分子力场函数。一般而言，分子力场函数由以下几个部分构成：
 
 * 键伸缩能：构成分子的各个化学键在键轴方向上的伸缩运动所引起的能量变化
 * 键角弯曲能：键角变化引起的分子能量变化
 * 二面角扭曲能：单键旋转引起分子骨架扭曲所产生的能量变化
 * 非键相互作用：包括范德华力、静电相互作用等与能量有关的非键相互作用
 * 交叉能量项：上述作用之间耦合引起的能量变化

 不同的分子力场会选取不同的函数形式来描述上述能量与体系构型之间的关系。

GAFF
-------------------------------------------------------

.. math::
    E_{pair} = & \sum_{bonds} K_r(r-r_{eq})^2 + \sum_{angles} K_{\theta}(\theta -\theta_{eq})^2 + \sum_{dihedrals} \frac{V_n}{2} [1 + \cos (n\phi-\gamma)] + \sum_{i<j} [\frac{A_{ij}}{R_{ij}^{12}} \\
    & - \frac{B_{ij}}{R_{ij}^6} + \frac{q_{i}q_{j}}{\varepsilon R_{ij}}]


AMBER
-------------------------------------------------------

.. math::
    E_{pot} = & \sum_{b} K_2(b-b_0)^2 + \sum_{\theta}H_{\theta}(\theta-\theta_0)^2 + \sum_{\phi} \frac{V_n}{2}[1 + \cos(n\phi-\phi_0)] \\
    & + \sum \varepsilon[(\frac{r^*}{r})^{12}-2(\frac{r^*}{r})^6] + \sum \frac{q_{i}q_{j}}{\varepsilon_{ij}r_{ij}} + \sum[\frac{C_{ij}}{r_{ij}^{12}}] + \sum[\frac{C_{ij}}{r_{ij}^{12}} - \frac{D_{ij}}{r_{ij}^{10}}]


CVFF
-------------------------------------------------------

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

PCFF
-------------------------------------------------------

.. math::
     E = \sum E^b + \sum E^a + \sum E^0 + \sum E^t + \sum E^{bb} + \sum E^{ab} + \sum E^{aa} + \sum E^{at} + \sum E^{bt} + \sum E^{elec} + \sum E^{VDW} \\
    & E^b = \sum_{i=2}^4 k_i^b (b-b_0)^i \\
    & E^a = \sum_{i=2}^4 k_i^a (\theta-\theta_0)^i \\
    & E^t = \sum_{i=1}^4 k_i^t (1-\cos{i\phi}) \\
    & E^0 = k^0 (\chi -\chi_0)^2 \\
    & \{E^{bb}, E^{aa}, E^{ab}\} = k^c (s-s_0)(s'-s'_0) \\
    & \{E^{bt}\} = (b-b_0)\sum_{i=1}^3 k_i^c (1-\cos{i\phi}) \\
    & \{E^{at}\} = (\theta-\theta_0)\sum_{i=1}^3 k_i^c (1-\cos{i\phi}) \\
    & E^{elec} = \sum_{ij} \frac{q_iq_j}{r_{ij}} \\
    & E^{VDW} = \sum_{ij} \epsilon_{ij}[2(\frac{r_{ij}^0}{r_{ij})^9 - 3(\frac{r_{ij}^0}{r_{ij})^6]

CFF
-------------------------------------------------------
.. math::
    E_{total} = & \sum_b [k_2(b-b_0)^2 + k_3(b-b_0)^3 + k_4(b-b_0)^4] + \sum_0 [k_2(\theta-\theta_0)^2 + k_3(\theta-\theta_0)^3 + k_4(\theta-\theta_0)^4] \\
                & +\sum_{\phi} [k_1(1-\cos \phi) + k_2(1-\cos2\phi) + k_3(1-\cos 3\phi)] + \sum_{\chi} k_2\chi^2 + \sum_{b,b'} k(b-b_0)(b'-b'_0) \\
                & +\sum_{b,\theta} k(b-b_0)(\theta-\theta_0) + \sum_{b,\phi} (b-b_0)[k_1\cos \phi + k_2\cos 2\phi + k_3\cos 3\phi] \\
                & +\sum_{\theta,\phi} k(\theta-\theta_0)[k_1\cos \phi + k_2\cos 2\phi + k_3\cos 3\phi] + \sum_{b,\theta} k(\theta'-\theta_0')(\theta-\theta_0) \\
                & + \sum_{\theta,\theta,\phi} k(\theta-\theta_0)(\theta'-\theta_0')\cos\phi + \sum_{i,j} \frac{q_iq_j}{r_{ij}} \\
                & +\sum_{i,j} \epsilon_{ij}[2(\frac{r_{ij}^0}{r_{ij}})^9 - 3(\frac{r_{ij}^0}{r_{ij}})^6] 

OPLS
-------------------------------------------------------
.. math::
    & E_{bond} = \sum_i k_{b,i}(r_i - r_{0,i})^2 \\
    & E_{bend} = \sum_i k_{\vartheta,i}(\vartheta_i - \vartheta_{0,i})^2 \\
    & E_{torsion} = \sum_i \{V_{1,i}(1 + \cos\phi_i)/2 + V_{2,i}(1 + \cos2\phi_i)/2 + V_{3,i}(1 + \cos3\phi_i)/2\} \\
    & E_{nb} = \sum_{i<j} \{\frac{q_iq_je^2}{r_{ij}} + 4\epsilon_{ij}[(\sigma_{ij}/r_{ij})^{12} - (\sigma_{ij}/r_{ij})^6]\} \\
    & \sigma_{ij} = \sqrt{\sigma_{ii}\sigma_{jj}} \\
    & \epsilon_{ij} = \sqrt{\epsilon_{ii}\epsilon_{jj}}

MMFF
-------------------------------------------------------
.. math::
    V_{total} = & \sum_{bonds} K_{bond}(r-r_{eq})^2(1+cs(r-r{eq}) + \frac{2}{7}(cs^2(r-r_{eq})^2)) \\
                & + \sum_{angle} K_{\theta}(\theta-\theta_{eq})^2(1+cb(\theta-\theta_{eq})) + \sum_{angle,linear} K_{al}(1+\cos(\theta)) \\
                & + \sum_{stretch,bend} (K_{ijk}(r_{ij}-r_{eq}) + K_{kji}(r_{kj}-r_{eq}))(\theta-\theta_{eq}) + \sum_{outofplane} K_{OOP}(\chi)^2 \\
                & + \sum_{dihedrals} \frac{V_1}{2}[1+\cos(\phi)] + \frac{V_2}{2}[1+\cos(2\phi)] + \frac{V_3}{2}[1+\cos(3\phi)] \\
                & + \sum_{i<j} [\epsilon(\frac{1.07\sigma}{r_{ij}+0.07\sigma})^7 (\frac{1.12\sigma^7}{r_{ij}^7+0.07\sigma^7}-2) - \frac{q_iq_j}{D(r_{ij}+\delta)}]

UFF
---------

  Bond:

1. Harmonic

.. math::
    E_R = 1/2K_{ij}(r-r_{ij})^2

2. Morse

.. math::
    E_R = D_{ij}[e^{-\alpha(r-r_{ij})}-1]^2
    \alpha = [\frac{k_{ij}}{2D_{ij}}]^{1/2}

  Angle:

.. math::
    E_{\theta} = \frac{K_{ijk}}{n^2}[1-\cos(n\theta)]

  Torsion:

.. math::
    E_{\phi} = 1/2V_{\phi}[1-\cos{n\phi_0}\cos{n\phi}]

  LJ:

.. math::
    E_{vdw} = D_{ij}\{-2[\frac{\chi_{ij}}{\chi}]^6 + [\frac{\chi_{ij}}{\chi}]^{12}\}

Dreiding
----------

  Bond:

1. Harmonic

.. math::
    E = 1/2K_e(R-R_e)^2

2. Morse

.. math::
    E = D_e[e^{-(\alpha nR-R_c)}-1]^2

  Angle:

.. math::
    E_{IJK} = K_{IJK}[1+\cos(\theta_{IJK})]

  Torsion:

.. math::
    E_{IJKL} = 1/2V_{JK}\{1-\cos[n_{JK}(\varphi-\varphi^0_{JK})]\}

  LJ:
  
.. math::
    E_{vdw}^{LJ} = AR^{-12}-BR^{-6}
    or E^{LJ} = D_0[\rho^{-12}-2\rho^{-6}]
    \rho = R/R_0

  LJ rules:

.. math::
    D_{oij} = [D_{oii}D_{ojj}]^{1/2}
    R_{oij} = 1/2(R_{oii}+R_{ojj})

However, Dreiding-X6:

.. math::
    R_{oij} = [R_{oii}R_{ojj}]^{1/2}

PCFF(polymer consistent force field)
----------------------------------------

.. math::
    E_{pot} = & \sum_{ij bonded} \sum_{n=2}^4 K_{rn,ij}(r_{ij}-r_{0,ij})^n + \sum_{ijk bonded} \sum_{n=2}^4 K_{\theta n,ijk}(\theta_{ijk}-\theta_{0,ijk})^n \\
              & + \sum_{ijkl bonded} \sum_{n=1}^3 V_{\phi n,ijkl}[1-\cos(n\phi_{ijkl}-\phi_{0n,ijkl})] \\
              & + \sum_{ijkl bonded} K_{\chi,ijkl}(\chi - \chi_{0,ijkl})^2 + E_{cross} \\
              & + \frac{1}{4\pi\epsilon_0\epsilon_r} \sum_{ij nonbonded} \frac{q_iq_j}{r_{ij}} \\
              & + \sum_{ij nonbonded} \epsilon_{0,ij} (2(\frac{r_{0,ij}}{r_{ij}})^9 - 3(\frac{r_{0,ij}}{r_{ij}})^6)

# GROMACS
------------

#   Covalent bond angles:


# .. math::
#     V(r_1,r_2,r_3) = 1/2 k_{\theta}(\theta - \theta_0)^2

#   Dihedral angles:

# .. math::
 #    V(r_1,r_2,r_3,r_4) = 1/2 V_0[1+\cos(n\phi - \phi_0)]

CFF93
-------

.. math::
    & E^b = \sum_{i=2}^4 k_i^b(b-b_0)^i \\
    & E^a = \sum_{i=2}^4 k_i^a(\theta-\theta_0)^i \\
    & E^t = \sum_{i=1}^4 k_i^t(1-\cos{i\phi}) \\
    & E^0 = k^0(\chi-\chi_0)^2 \\
    & \{E^{bb}, E^{aa}, E^{ab}\} = k^c(s-s_0)(s'-s'_0) \\
    & \{e_{bt}\} = (b-b_0) \sum_{i=1}^3 k_i^c(1-\cos{i\phi}) \\
    & \{e_{at}\} = (\theta-\theta_0) \sum_{i=1}^3 k_i^c(1-\cos{i\phi}) \\
    & E_{ec}^{el} = \sum_{ij}\frac{q_iq_j}{r_{ij}} \\
    & E^{VDW} = \sum_{ij} \epsilon_{i,j} [2(\frac{r_{ij}^0}{r_{ij}})^9 - 3(\frac{r_{ij}^0}{r_{ij}})^6]

CFF91
--------

.. math::
    V = & \sum_{bonds}D_b[1-e^{-\alpha(b-b_0)}]^2 = \sum_{angles}H_{\theta}(\theta-\theta_0)^2 \\
        & + \sum_{out of plane}H_{\chi}\chi^2 + \sum_{torsion}H_{\tau}(1-s\cos{n\tau}) \\
        & + \sum_{bb'}F_{bb'}(b-b_0)(b'-b'_0) + \sum_{b\theta}F_{b\theta}(b-b_0)(\theta-\theta_0) \\
        & + \sum_{\theta\theta'}F_{\theta\theta'}(\theta-\theta_0)(\theta'-\theta'_0) \\
        & + \sum_{\chi\chi'}F_{\chi\chi'}\chi\chi' \\
        & + \sum_{\tau\theta\theta'}F_{\tau\theta\theta'}\cos{\tau}(\theta-\theta_0)(\theta'-\theta'_0) \\
        & + \sum_{nonbond}\{-4\epsilon[(\frac{r^{\ast}}{r})^{12} - (\frac{r^{\ast}}{r})^{6}] + \frac{q_1q_2}{r}\}

CLAYFF
----------

.. math::
    & E_{total} = E_{coulombic} + E_{vdw} + E_{bond stretch} + E_{angle bend} \\
    & E_{coulombic} = \frac{e^2}{4\pi\epsilon_0}\sum_{i\neq j}\frac{q_iq_j}{r_{ij}} \\
    & E_{vdw} = \sum_{i\neq j}D_{0,ij}[[\frac{R_{0,ij}}{r_{ij}}]^{12}-2(\frac{R_{0,ij}}{r_{ij}})^6] \\
    & E_{bond stretch} = \sum_{bonds} k_1(r_{ij}-r_0)^2 \\
    & E_{angle bend} = \sum_{angles}k_2(\theta_{ijk}-\theta_0)^2 \\
    & R_{o,ij} = 1/2(R_{o.i} + R_{o,j}) \\
    & D_{o,ij} = \sqrt{D_{o,i}D_{o,j}}

GROMOS-53A5 and 53A6
-----------------------

* Covalent Bond Interactions

.. math::
    V^{bond}(r;s) = V^{bond}(r;K_b,b_0) = \sum_{n=1}^{N_b} 1/4 K_{bn}[b_n^2 - b_{0n}^2]^2

* Covalent Bond-Angle Interactions

.. math::
    & V^{angle}(r;s) = V^{angle}(r;K_{\theta},\theta_0) = \sum_{n=1}^{N_{\theta}} 1/2 K_{\theta_n}[\cos{\theta_n}-\cos{\theta_{0n}}]^2 \\
    & K_{\theta n} = \frac{2K_BT}{[\cos(\theta_{0n} + \sqrt{\frac{k_B T}{K_{\theta n}^{harm}}}) - \cos{\theta_{0n}}]^2 + [\cos(\theta_{0n} - \sqrt{\frac{k_B T}{K_{\theta n}^{harm}}})-\cos{\theta_{0n}}]^2}
    
* Improper Dihedral-Angle Interactions

.. math::
    & V^{har}(r;s) = V^{har}(r;K_{\xi},\xi_0) = \sum_{n=1}^{N_{\xi}} 1/2 K_{\xi n}[\xi_n - \xi_{0n}]^2 \\
    & \xi_n = sign(\xi_n)arccos(\frac{r_{mj}\bullet r_{qk}}{r_{mj}r_{qk}})

* Torsional Dihedral-Angle Interactions

.. math::
    V^{trig}(r;s) = V^{trig}(r;K_{\varphi},\delta,m) = \sum_{n=1}^{N_{\varphi}}K_{\varphi n}[1+\cos(\delta_n)\cos(m_n\varphi_n)]

* Van der Waals Interactions

.. math::
    & V^{LJ}(r;s) = V^{LJ}(r;C12,C6) = \sum_{pairs\ i,j}(\frac{C12_{ij}}{r_{ij}^{12}} - \frac{C6_{ij}}{r_{ij}^{6}}) \\
    & C12_{ij} = \sqrt{C12_{ij}\bullet C12_{jj}} \\
    & C6_{ij} = \sqrt{C6_{ij}\bullet C6_{jj}}

* Electrostatic Interactions

.. math::
    V^C(r;s) = V^C(r;q) = \sum_{pairs\ i,j} \frac{q_iq_j}{4\pi\epsilon_0\epsilon_1}\frac{1}{r_{ij}}

CHARMM
------------

.. math::
    U(\vec{R}) = & \underbrace{\sum_{bonds}k_i^{bond}(r_i-r_0)^2}_{U_{bond}} + \underbrace{\sum_{angles}k_i^{angle}(\theta_i-\theta_0)^2}_{U_{angle}} \\
                 & + \underbrace{\sum_{dihedrals}k_i^{dihe}[1+\cos{n_i\phi_i+\delta_i}]}_{U_{dihedral}} \\
                 & + \underbrace{\sum_{i}\sum_{j\neq i}4\epsilon_{ij}[(\frac{\sigma_{ij}}{r_{ij}})^{12}-(\frac{\sigma_{ij}}{r_{ij}})^6]+\sum_i\sum_{j\neq i}\frac{q_iq_j}{\epsilon r_{ij}}}_{U_{nonbond}}

.. math::
    & \sigma_{ij} = \frac{\sigma_{ii}+\sigma_{jj}}{2} \\
    & \epsilon_{ij} = \sqrt{\epsilon_{ii}\epsilon_{jj}}

CGenFF
-------------

* Intramolecular(internal, bonded terms)

.. math::
    & \sum_{bonds}K_b(b-b_0)^2 + \sum_{angles}K_{\theta}(\theta-\theta_0)^2 \\
    & + \sum_{dihedrals}K_{\phi}(1+\cos(n\phi-\theta)) + \sum_{improper\ dihedrals}K_{\varphi}(\varphi-\varphi_0)^2 \\
    & \sum_{Urey-Bradtey}K_{UB}(r_{1,3}-r_{1,3;0})^2

* Intermolecular(external, nonbonded terms)

.. math::
    \sum_{nonbonded}\frac{q_iq_j}{4\pi Dr_{ij}} + \epsilon_{ij}[(\frac{R_{min,ij}}{r_{ij}})^{12}-2(\frac{R_{min,ij}}{r_{ij}})^6]

