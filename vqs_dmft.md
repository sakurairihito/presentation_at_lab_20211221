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


  ![bg  center](drawio/sample4.drawio.svg)
  <!--![80% bg right:50%](./drawio/wataru_mizukami.svg)-->
  
  **Today's talk is based on following preprint**
  > ## **Hybrid quantum-classical algorithm for computing imaginary-time correlation functions**
  > <https://arxiv.org/abs/2112.02764>

  **collaborator**
   Wataru Mizukami (QIQB, Oska Univ.),
   Hiroshi Shinaoka (Saitama Univ.)

---
# Computational materials science

- DFTあたりから始める
- 単一のスレーター行列で状態を近似
- 半導体、金属の電子状態の記述は成功
- 強相関だと量子的な重ね合わせやエンタングルメントの増加により、DFT単体では記述が難しい
- なぜDMFTなのか？
- 固体の一部の自由度に着目し、相関のある不純物とその周りの環境の影響を考慮することで、固体の計算にアプローチする手法

---
# Dynamical mean-field theory (DMFT)

![bg center width:140% height:135%](drawio/dmft.drawio.svg)

- 
- The biggest bottle neck: **quantum impurity problem (Computing Green's function)**
- 
- No efficient method for computing imaginary-time 
Green's functions
- ddddd

---
# Solving impurity problems with Quantum computer
#### Foult-tolerant quantum computer

- Quantum phase estimation algorithm (reference:)

#### Quantum devices with limited hardware resources 
- e.g.) Noisy Intermediate Scale Devices (NISQ) 
noisy quantum devices with ~100 qubits, about 100 depth (# of time steps)

- 
- No efficient method for computing imaginary-time 
Green's functions
- ddddd


---
# Imaginary-time Green's function 

<!--![bg center width:140% height:135%](drawio/dmft.drawio.svg)-->

##### Hamiltonian

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
# Outline of our algorithm 

<!--文字を上から書くようにする。デフォルトだと中央から書き始める。-->
<style scoped>
    section {
        justify-content: start;
    }
</style>



-  Introduce a fine mesh of $\tau$ in [-$\beta$ / 2, $\beta$ / 2]  
-  Then, compute $G_{ij}(\tau)$ 

$$
\begin{aligned}
G_{i j}(\tau) &=-\operatorname{Tr}\left[e^{-\beta \hat{H}} c_{i}(\tau) c_{j}^{\dagger}(0)\right] / \operatorname{Tr}\left(e^{-\beta \mathcal{H}}\right) \\
& \simeq-\underbrace{\left\langle\Phi_{G S}(0)\right| e^{-(\beta-\tau) \hat{H}}}_{\left\langle\Phi_{C}^{\prime}\right|}  c_{i}(0)\underbrace{e^{-\tau \hat{H}} c_{j}^{\dagger}(0)\left|\Phi_{G S}(0)\right\rangle}_{\left|\Phi_{C}\right\rangle}/\left(e^{-\beta E_{G}}\right)
\end{aligned}
$$

![bg center width:140% height:135%](drawio/stage.drawio.svg)

---
# Ground-state calculation 

<style scoped>
    section {
        justify-content: start;
    }
</style>


### Preparation
- The hamiltonian need to be transformed to the qubit representation 
  e.g.) Jordan-Wigner transformation
$$
\begin{aligned}
H & \rightarrow \sum_{p}^{} h_{p} S_{p},  S_{p}\in\{X, Y, Z, I\}^{\otimes m}
\end{aligned}
$$

### Variational Quantum Eigensolver (VQE)

- Selection of anzatz 
- 0ptimization
(Gradient based opt or 
parameter-shift rule 

![bg center width:120% height:118%](drawio/vqe.drawio.svg)

---
# Single-particle excitation

<!--![bg center width:120% height:118%](drawio/trans_amp.drawio.svg)-->
![bg right:49% contain](zu_svg_jpg/transition_amplitude_2.svg)
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
# Imaginary-time evolution



---
# Transition amplitude 


---
## Conclution
:thumbsup:A new hybrid quantum classical algorithm for computing imaginary-time Green's functions by applying the VQS
:thumbsup:Out algorithms efficiently computes GF using non-uniform mesh based on IRbasis 
:thumbsdown:The hardware cost for transition amplitude is high for NISQ devices ([link of slide2](#2))
:bulb:[Classical shadow](https://arxiv.org/abs/2110.02965) may be the savior in the age of NISQ era.


####  future plan
:one:**noise model**: 〜〜〜
:two:**compact ansatz**:  〜〜〜

---
# Marpを使った感想


### 課題
- デフォルトで、図を好きな枚数・位置に貼り付けができない:crying_cat_face:
- 今回は、drawioというアプリを使って、特定位置へ図貼り付けをした


### 欲しい機能
- スライドの枚数を全て表示させたい
- 文字の背景に色をつけたい
- ~~文字のフォントをTimes New Romanにしたい。~~
- pdfでも貼り付けられるようにしてほしい（or pdf --> svg, jpgへの変換ツールの充実）

---

### 数式

$$I_{xx}=\int\int_Ry^2f(x,y)\cdot{}dydx$$

Yahoo!!

---
Inline $\sqrt{3x-1}+(1+x)^2$

Block

$$
\begin{array}{c}

\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &
= \frac{4\pi}{c}\vec{\mathbf{j}}    \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\

\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\

\nabla \cdot \vec{\mathbf{B}} & = 0

\end{array}
$$

---


## <!-- fit --> 数式と画像をいれる

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

![60% bg left:50%](drawio/sample2.drawio.svg)

$$
\begin{array}{c}

\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &
= \frac{4\pi}{c}\vec{\mathbf{j}}    \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\

\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\

\nabla \cdot \vec{\mathbf{B}} & = 0

\end{array}
$$

---
#### Imaginary-time Green's function

$$
\begin{aligned}
G_{i j}(\tau) &=-\operatorname{Tr}\left[e^{-\beta \hat{H}} c_{i}(\tau) c_{j}^{\dagger}(0)\right] / \operatorname{Tr}\left(e^{-\beta \mathcal{H}}\right) \\
& \simeq-\underbrace{\left\langle\Phi_{G S}(0)\right| e^{-(\beta-\tau) \hat{H}}}_{\left\langle\Phi_{C}^{\prime}\right|}  c_{i}(0)\underbrace{e^{-\tau \hat{H}} c_{j}^{\dagger}(0)\left|\Phi_{G S}(0)\right\rangle}_{\left|\Phi_{C}\right\rangle}/\left(e^{-\beta E_{G}}\right)
\end{aligned}
$$

$ax^2+bx+2$
*dddd*
**dddd**
dddd<br />
dddd


---

- fff
''''
   ffff
'''' 
{.info}
:   これはアイコンなしの付箋です。__
    余白も小さくコンパクトになります。

{.note} メモ
:   Markdownはとても便利です。␣␣
    複数行に分けて書くこともできます。

<div style="text-align:center">中央寄せにしたい文章</div>

---

```
package helloworld;

public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
`インラインコード`