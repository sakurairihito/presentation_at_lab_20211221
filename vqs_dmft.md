---
marp: true
theme: gaia
size: 16:9
paginate: true
#headingDivider: 2
header: ""
footer: ""
---


<!--
_class: lead
_paginate: false
_header: ""
_footer: © 2021 12/21 Seminor @ condensed matter theory of Saitama University
-->


### **Hybrid quantum-classical algorithm for computing imaginary-time correlation functions**

 **Today's talk is based on the following preprint**
 <https://scirate.com/arxiv/2112.02764>

**Rihito Sakurai, Ph.D. student of the Graduate School of Science & Engineering, Saitama University**

![bg blur::10px　opacity:0.2 saturate:3](./circle.jpg)



---
1. [数式](#3)
2. [inline](#4)
3. [左に画像を入れる](#5)


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

![80% bg left:50%](./sample.drawio.svg)

#### 左に画像をいれる

- 表示場所、比率を指定する
- 次頁では、複数画像を並べます
- footerで画像クレジット表示も
- footer



---

## <!-- fit --> 数式と画像をいれる

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

![60% bg left:50%](./sample2.drawio.svg)

$$
\begin{array}{c}

\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &
= \frac{4\pi}{c}\vec{\mathbf{j}}    \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\

\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\

\nabla \cdot \vec{\mathbf{B}} & = 0

\end{array}
$$

---
## <!-- fit --> Matsubara Green's function

<span style="font-size: 100%;">もじもじ</span>

![100% bg right:50%](./sample3.drawio.svg)





---

#### Matsubara Green's function

![height::500px](./sample3.drawio.svg)

<style scoped>
    section {
        text-align: center;
    }
</style>
このスライドのみ、中央揃えに設定した
ddd
ddddd

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



<div style="text-align:center">中央寄せにしたい文章</div>

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
# ほしい機能

-スライドの枚数を全て表示させたい
-文字の背景に色をつけたい
-文字のフォントをTimes New Romanにしたい。

---
#### Dynamical mean-field theory (DMFT)

![100% bg right:45%](./dmft.drawio.svg)

- bottle neck:**quantum impurity problem**
- 
- No efficient method for computing imaginary-time Green's functions
- ddddd