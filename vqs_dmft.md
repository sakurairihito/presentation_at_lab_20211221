---
marp: true
theme: default
size: 16:9
paginate: true
#headingDivider: 2
header: '[R. Sakurai, W. Mizukami, H. Shinaoka, arXiv:2112.02764](<https://arxiv.org/abs/2112.02764>)'
footer: ""
---

<!--タイトルの位置,headerを固定化。 タイトルはpositionを左から25px,上から20pxの場所に固定化する-->
<!--全体に影響-->
<style>
    h1{
        position: absolute;
        left: 10px; top: 0px;
    }
    header {
  top: 20px;
  left: 830px;
}
    section { font-size: 160%; }
</style>




<!--このスライドのみ以下を適用-->
<!--
_class: lead
_paginate: false
_header:   ""
_footer: @物性理論セミナー　Date: 2021.12.21
-->

<!--このスライドだけタイトル以外の文字を中央揃えにする。-->
<style scoped>
    section {
        text-align: center;
    }
</style>

<!-- 以下よりスライドの始まり-->
## **Hybrid quantum-classical algorithm for computing imaginary-time correlation functions**
  Rihito Sakurai
  (Ph.D. student of the Graduate School of Science & Engineering, Saitama University)


---

<!--このスライドだけタイトル以外の文字を中央揃えにする。-->
<style scoped>
    section {
        text-align: center;
    }
</style>

New preprint
===========


  <!--![bg  center](drawio/sample4.drawio.svg)-->
  <!--![80% bg right:50%](./drawio/wataru_mizukami.svg)-->
  
  **Today's talk is based on following preprint**
  > ## **Hybrid quantum-classical algorithm for computing imaginary-time correlation functions**
  > <https://arxiv.org/abs/2112.02764>

  **collaborator**
   Wataru Mizukami (QIQB, Oska Univ.),
   Hiroshi Shinaoka (Saitama Univ.)

---
# Computational materials science

- 密度汎関数理論 (DFT)の概略とその問題点についてまとめる 
- 単一のスレーター行列で物質の電子状態を近似
- 多くの半導体、金属の電子状態の記述は成功
- 強相関だと量子的な重ね合わせやエンタングルメントの増加により、DFT単体では記述が難しい


---
# Dynamical mean-field theory (DMFT)

![bg right width:160% height:165%](drawio/dmft.drawio.svg)


- effective bath parameters are determined from the self-consistent condition:
$$
G_{\mathrm{imp}}=G_{\mathrm{loc}}^{\mathrm{lattice}} \equiv \sum_{\boldsymbol{k}} \frac{1}{i \omega_{n}-\left(\epsilon_{\boldsymbol{k}}-\mu\right)-\Sigma}
$$


- The biggest bottle neck: **quantum impurity problem (Computing Green's function)**
- Classical methods: Quantum monte carlo, Tensor network
- Solving impurity models wit multi-orbital and multi impurity sites is challenging task
---


# Solving impurity problems with Quantum computer

<!--このスライドのみ以下を適用-->
<!--
_class: lead
_paginate: false
_header:   ""
_footer:   ""
-->


#### Foult-tolerant quantum computer

- Quantum phase estimation algorithm 
- It assumes fault-tolerant QC

#### Quantum devices with limited hardware resources 
- e.g.) Noisy Intermediate Scale Devices (NISQ) 
noisy quantum devices with ~100 qubits, about 100 depth (# of time steps)
- need to calculate expectation value of the square of the Hamiltonian 
(H. Chen et _al_., arXiv :2105.01703v2) 
- Efficient methods for computing imaginary-time Green's functions need to be explored
- Our work: new algorithm to compute the imaginary-time Green's function


---
# Imaginary-time Green's function 

<!--![bg center width:140% height:135%](drawio/dmft.drawio.svg)-->

##### Hamiltonian
<style scoped>
    section {
        justify-content: start;
    }
    section { font-size: 170%; }
</style>


  $${H} = \sum_{ij}^N t_{ij} \hat c^\dagger_i \hat c_j + \frac{1}{4} \sum_{ijkl} U_{ikjl}\hat c_i^\dagger \hat c_j^\dagger \hat c_l \hat c_k - \mu \sum_i \hat c^\dagger_i \hat c_i,$$

$c_i/c^\dagger_i$ : the creation and annihilation operators for the spin orbital $i$

##### Imaginary-time Green's function (GF)

$$G_{a b}(\tau)=-\theta(\tau)\left\langle\hat{c}_{a}(\tau) \hat{c}_{b}^{\dagger}(0)\right\rangle + \theta(\tau) \left\langle\hat{c}_{b}^{\dagger}(0) \hat{c}_{a}(\tau)\right\rangle, \tau = it, \hbar=k_{\mathrm{B}}=1,$$

- At sufficiently low temperature $T$
$$G_{a b}(\tau) \underset{T \rightarrow 0}{=} \mp\left\langle\Psi_{\mathrm{G}}\left|\hat{A}_{\pm} e^{\mp\left(\mathcal{H}-E_{\mathrm{G}}\right) \tau} \hat{B}_{\pm}\right| \Psi_{\mathrm{G}}\right\rangle,  |\Psi_{\mathrm{G}}\rangle:\text{ground state}$$

$$
A_{+}=\hat{c}_{a} \text { and } B_{+}=\hat{c}_{b}^{\dagger} \text { for } 0 < \tau <\frac{\beta}{2}, 
 A_{-}=\hat{c}_{b}^{\dagger} \text { and } B_{+}=\hat{c}_{a} \text { for } -\frac{\beta}{2}<\tau<0
,  (\beta=1/T)
$$




---

- 実例として、ハバードアトムなどのグリーン関数の図を見せる(上のスライド）)
- 指数関数
- 数式との対応づける。
- N --> N+1 --> N
- 虚時間が
- Non-uniform なメッシュの図
- correction vectorの先行研究と何が非自明なのか。
- $\beta$: fictious temperature
- 応答関数で記述される物理量（スペクトル関数とか格子スピン感受率）が物性では重要
- DMFT  
- 複数不純物クラスターの低温での計算は困難。
- 将来的に、量子で
- 有限温度の
---


# Outline of our algorithm 

<style scoped>
    section {
        justify-content: start;
    }
    section { font-size: 170%; }
</style>


<!--文字を上から書くようにする。デフォルトだと中央から書き始める。-->
<style scoped>
    section {
        justify-content: start;
    }
</style>



-  Introduce a fine mesh of $\tau$ in [-$\beta$ / 2, $\beta$ / 2]  
-  Then, compute $G_{ij}(\tau)$ 

For $\tau$>0, 
$$
\begin{aligned}
G_{i j}(\tau) &=-\operatorname{Tr}\left[e^{-\beta \hat{H}} c_{i}(\tau) c_{j}^{\dagger}(0)\right] / \operatorname{Tr}\left(e^{-\beta \mathcal{H}}\right) \\
& \simeq-\underbrace{\left\langle\Phi_{G}(0)\right| e^{-(\beta-\tau) \hat{H}}}_{\left\langle\Phi_{C}^{\prime}\right|}  c_{i}(0)\underbrace{e^{-\tau \hat{H}} c_{j}^{\dagger}(0)\left|\Phi_{G}(0)\right\rangle}_{\left|\Phi_{C}\right\rangle}/\left(e^{-\beta E_{G}}\right)
\end{aligned}
$$

![bg center width:140% height:135%](drawio/stage.drawio.svg)

---
# STAGE1: Ground-state calculation 

<style scoped>
    section {
        justify-content: start;
    }
    section { font-size: 170%; }
</style>


### Preparation
- The hamiltonian need to be transformed to the qubit representation 
  e.g.) Jordan-Wigner transformation
$$
\begin{aligned}
H & \rightarrow \sum_{p}^{} h_{p} S_{p},  S_{p}\in\{X, Y, Z, I\}^{\otimes m}
\end{aligned}
$$

### Variational Quantum Eigensolver (VQE) ------>
:small_blue_diamond:**Optimization**: parameter-shift rule
 <https://arxiv.org/pdf/1803.00745.pdf>
 

$\left.\frac{\partial\langle H(\theta)\rangle}{\partial \theta_{i}}=\frac{1}{2}\left( H\left(\theta+\frac{\pi}{2} e_{i}\right)\right)-\left\langle H\left(\theta-\frac{\pi}{2} e_{i}\right)\right\rangle\right)$
$(U(\boldsymbol{\theta})=\prod_{k} e^{-i \theta_{k} P_{k} / 2})$


![bg center width:120% height:118%](drawio/vqe.drawio.svg)


---
# STAGE2: Single-particle excitation

<style scoped>
    section {
        justify-content: start;
    }
    section { font-size: 170%; }
</style>


<!--![bg center width:120% height:118%](drawio/trans_amp.drawio.svg)-->
![bg right:51% contain](zu_svg_jpg/transition_amplitude_2.svg)
<style scoped>
    section {
       text-align: left;
    }
</style>

<!--
 $\hat{c}_{a}^{\dagger}\left|\Psi_{\mathrm{GS}}\right\rangle \simeq c_{1}\left|\phi_{\mathrm{EX}}\left(\vec{\theta}_{\mathrm{EX}}\right)\right\rangle$ for $\tau>0$
 we compute the coefficient $c_1$ and $\vec{\theta}_{\mathrm{EX}}^{*}$ according to following steps.
 -->

#####
For $\tau>0$, 
$\hat{c}_{a}^{\dagger}\left|\Psi_{\mathrm{GS}}\right\rangle \simeq c_{1}\left|\phi_{\mathrm{EX}}\left(\vec{\theta}_{\mathrm{EX}}\right)\right\rangle$ 

1. single-particel excited state
$c_{a}^{\dagger}\left|\Psi_{G}\right\rangle=\frac{X_{a}-i Y_{a}}{2} Z_{a-1} \ldots Z_{1}\left|\Psi_{G}\right\rangle$


2. Prepare $\left|\phi_{\mathrm{EX}}\left(\vec{\theta}_{\mathrm{EX}}\right)\right\rangle$ and measure $\left\langle\phi_{\mathrm{EX}}\left(\vec{\theta}_{\mathrm{EX}}\right)\left|\hat{c}_{a}^{\dagger}\right| \Psi_{\mathrm{G}}\right\rangle$

3. Minimize cost function: 
$|1-\langle\Psi_{EX}(\vec{\theta})| c_{a}^{\dagger}\left|\Psi_{G}\right\rangle|^2$

4. Measure 
$c_{1}=\left\langle\phi_{\mathrm{EX}}\left(\vec{\theta}_{\mathrm{EX}}^{*}\right)\left|c_{a}^{\dagger}\right| \Psi_{\mathrm{G}}\right\rangle$



---
# STAGE3: Imaginary-time evolution

<style scoped>
    section {
        justify-content: start;
    }
    section { font-size: 143%; }
</style>


- The time-dependent Schrödinger equation

$\frac{\mathrm{d}}{\mathrm{d} \tau}|\tilde{\Psi}(\tau)\rangle=-\left({H}-E_{\tau}\right)|\tilde{\Psi}(\tau)\rangle$

$|\tilde{\Psi}(\tau)\rangle \equiv|\Psi(\tau)\rangle / \sqrt{\langle\Psi(\tau) \mid \Psi(\tau)\rangle}, E_{\tau}\equiv\langle\tilde{\Psi}(\tau)|{H}| \tilde{\Psi}(\tau)\rangle$

- Prepare the following state on a quantum computer
$|\tilde{\Psi}(\tau)\rangle=|\phi(\vec{\theta}(\tau))\rangle$
$|\Psi(\tau)\rangle=e^{\eta(\tau)}|\phi(\vec{\theta}(\tau))\rangle -(1)$

- Introduce Norm $e^{\eta(\tau)}$ : 
$\frac{\mathrm{d}}{\mathrm{d} \tau}|\Psi(\tau)\rangle=-{H}|\Psi(\tau)\rangle -(2)$ 
- From (1) and (2), 
$\frac{d\eta(\tau)}{d\tau}=-E_{\tau} , (\eta \in \mathbb{R}  \text{ and} \frac{\mathrm{d}\langle\Psi \mid \Psi\rangle}{\mathrm{d} \tau}=0)$

Question: How do we determine $\vec{\theta}(\tau)$ on a discrete mesh of $\tau$ ?
**McLachlan’s variational principal**  (A. McLachlan, Mol. Phys 8, 39-44 (1996))
$$\min \delta\left\|\left(\frac{\mathrm{d}}{\mathrm{d} \tau}+{H}-E_{\tau}\right) \mid \phi(\vec{\theta}(\tau))\rangle\right\|$$




---
# STAGE3: Imaginary-time evolution

<style scoped>
    section {
        justify-content: start;
    }
    section { font-size: 150%; }
</style>

#### Varuational Quantum Simulation (VQS)
$\min \delta\left\|\left(\frac{\mathrm{d}}{\mathrm{d} \tau}+{H}-E_{\tau}\right) \mid \phi(\vec{\theta}(\tau))\rangle\right\|$

$\sum_{j} A_{i j} \dot{\theta}_{j}= C_{i}$

$\begin{aligned} A_{i j} & \equiv \mathcal{R} \frac{\partial\langle\phi(\vec{\theta})|}{\partial \theta_{i}} \frac{\partial|\phi(\vec{\theta})\rangle}{\partial \theta_{j}},  C_{i} & \equiv-\mathcal{R}\langle\phi(\tau)| \mathcal{H} \frac{\partial|\phi(\vec{\theta})\rangle}{\partial \theta_{i}} \end{aligned}$

$\vec{\theta}(\tau+\Delta \tau) \simeq \vec{\theta}(\tau)+A^{-1} \vec{C} \Delta \tau$



#### Direct VQS
$
\begin{aligned}
&\theta(\tau+\Delta \tau) \\
&\simeq \underset{\vec{\theta}}{\operatorname{argmin}} \||\phi(\vec{\theta})\rangle-|\Psi(\tau)\rangle+\Delta \tau\left(\mathcal{H}-E_{\tau}\right)|\Psi(\tau)\rangle \|
\end{aligned}
$
$=\underset{\vec{\theta}}{\operatorname{argmin}} \operatorname{Re} \mid \Delta \tau\langle\phi(\vec{\theta})|H| \Psi(\tau)\rangle-\left(\Delta \tau E_{\tau}+1\right)\langle\phi(\vec{\theta}) \mid \Psi(\tau)\rangle$



---
# STAGE4: Transition amplitude 
![bg right:48% contain](zu_svg_jpg/transition_amplitude_2.svg)
transition amplitude
$\left\langle\Psi_{\mathrm{G}}\left|A_{\pm}\right| \Psi_{\mathrm{IM}}\right\rangle$

-->$G(\tau)
=-c_{1} e^{\eta(\tau)} e^{\tau E_{\mathrm{G}}}\left\langle\Psi_{\mathrm{G}}\left|A_{\pm}\right| \Psi_{\mathrm{IM}}\right\rangle$




---
# Numerical Details

<style scoped>
  
    section {
        justify-content: start;
        text-align: ;
    }
    section { font-size: 150%; }
</style>

#### Ansatz
- Unitary coupled cluster with generalized singles and doubles (UCCGSD)
(Nooijen, Marcel. Phys. Rev. Lett. **84**, 2108 (2000), 
J. Lee, _et al_., Journal  of  chemical  theory and computation **15**, 311 (2019))
- $U(\boldsymbol{\theta})=
 \prod_{i, j, a, b=1}^{n}\left\{e^{\theta_{i j}^{a b} a_{a}^{\dagger} a_{b}^{\dagger} a_{j} a_{i}- \theta_{i j}^{a b}a_{i}^{\dagger} a_{j}^{\dagger} a_{b} a_{a}}\right\} \prod_{a, i=1}^{n}\left\{e^{\theta_{i}^{a} a_{a}^{\dagger} a_{i}-\theta_{i}^{a } a_{i}^{\dagger} a_{a}}\right\}$

- $N_p:$ # of the parameters ~ $O\left(n_{}^{4}\right)$

#### Optimization
- A quasi-Newton method (BFGS method)


#### Non-uniform mesh of $\tau$ in $[-\beta / 2, \beta / 2]$ ($\beta=1000$)
- A sparse mesh generated according to the intermediate-representation (IR) basis. 
(J. Li _et al_.,  PRB **101**, 035144 (2020))

---
# Results: Dimer model


<style scoped>
  
    section {
        justify-content: start;
        text-align: ;
    }
    section { font-size: 150%; }
</style>


![bg center width:120% height:165%](drawio/dimer_model.drawio.svg)
<!--![bg center width:140% height:116%](zu_svg_jpg/dimer_model.svg)-->
 
$
\begin{aligned}
\mathcal{H} &=U \hat{n}_{1 \dagger} \hat{n}_{1 \downarrow}-\mu \sum_{\sigma=\uparrow, \downarrow} \hat{n}_{1 \sigma} -V \sum_{\sigma=\uparrow, \downarrow}\left(\hat{c}_{1 \sigma}^{\dagger} \hat{c}_{2 \sigma}+\hat{c}_{2 \sigma}^{\dagger} \hat{c}_{1 \sigma}\right)+\epsilon_{b} \sum_{\sigma=\uparrow, \downarrow} \hat{n}_{2 \sigma}
\end{aligned}
$

- 79 sparse sampling points 
- Non-diagonal componet also can be computed

---

<style scoped>
  
    section {
        justify-content: start;
        text-align: ;
    }
    section { font-size: 150%; }
</style>


# Results : Four-site model 
![bg center width:110% height:145%](drawio/foursite_model.drawio.svg)

$
\begin{aligned}
H &=U \hat{n}_{0 \uparrow} \hat{n}_{0 \downarrow}-\mu \sum_{\sigma=\uparrow, \downarrow} \hat{n}_{0 \sigma} -\sum_{k=1}^{3} \sum_{\sigma=\uparrow, \downarrow} V_{k}\left(\hat{c}_{0 \sigma}^{\dagger} \hat{c}_{k \sigma}+\hat{c}_{k \sigma}^{\dagger} \hat{c}_{0 \sigma}\right)+\epsilon_{k} \sum_{k=1}^{3} \sum_{\sigma=\uparrow, \downarrow} \hat{n}_{k \sigma}
\end{aligned}
$

- At half-filling
- 70 sparse sampling points + adaptive construction of the mesh


---
<style scoped>
  
    section {
        justify-content: start;
        text-align: ;
    }
    section { font-size: 150%; }
</style>


# Results : Fourier-transformed Green's function

- We use irbasis (N. Chikano _et al_., Comput. Phys. Commun. **240**, 181 (2019))
- 


![bg center width:110% height:140%](drawio/fourier.drawio.svg)

<!--ここにコメントアウトした文字が反映される！-->
<!--$\tau$-->

---
## Conclution
- New hybrid quantum classical algorithm for computing imaginary-time Green's functions by applying the VQS
- Out algorithms efficiently computes GF using non-uniform mesh based on IRbasis 
- No need to calculate expectation value of the square of the Hamiltonian 
(H. Chen et _al_., arXiv :2105.01703v2)
- The hardware cost for transition amplitude may be high for NISQ devices :disappointed_relieved:


##  future plan
:one:**Simulation under realistic noise model**: 2 qubit error
:two:**Ansatz for impurity models**: tensor decomposition, topology of impurity models

