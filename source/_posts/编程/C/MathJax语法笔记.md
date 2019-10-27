---
title: MathJax语法笔记
author: 陈德强
date: 2019-10-24 21:29:00
categories: 笔记
tags: 其他
toc: true
top: 
mathjax: true
img: /medias/paperimg/night.jpg
summary: MathJax语法笔记。
---




# 如何插入公式
LaTeX的数学公式有两种：行中公式和独立公式。行中公式放在文中与其它文字混编，独立公式单独成行。

行中公式可以用如下三种方法表示：

`＼(数学公式＼)` 　
`$数学公式$`
`（数学公式）`,此时小括号失效，化身为公式命令，要想显示小括号，就要使用`$数学公式$`显示

独立公式可以用如下两种方法表示：

`＼[数学公式＼]`　
`$$数学公式$$`

例子：
`
\$[J_\alpha(x) = \sum_{m=0}^\infty \frac{(-1)^m}{m! \Gamma (m + \alpha + 1)} {\left({ \frac{x}{2} }\right)}^{2m + \alpha}\]$
`

显示：\$[J_\alpha(x) = \sum_{m=0}^\infty \frac{(-1)^m}{m! \Gamma (m + \alpha + 1)} {\left({ \frac{x}{2} }\right)}^{2m + \alpha}\]$

# 如何输入上下标
^表示上标, _表示下标。如果上下标的内容多于一个字符，要用{}把这些内容括起来当成一个整体。上下标是可以嵌套的，也可以同时使用。

栗子1：`$$x^{y^z}=(1+{\rm e}^x)^{-2xy^w}$$`
$$x^{y^z}=(1+{\rm e}^x)^{-2xy^w}$$


栗子2：`$$\max_{k}$$`
$$\max_{k}$$

# 希腊字母

|显示|	命令|	显示|	命令|
|:---:|:---:|:---:|:---:|
α	|\alpha	|β	|\beta
γ	|\gamma	|δ	|\delta
ε	|\epsilon|	ζ|	\zeta
η	|\eta	|θ|	\theta
ι	|\iota	|κ|	\kappa
λ	|\lambda	|μ|	\mu
ν	|\nu	|ξ	|\xi
π	|\pi	|ρ	|\rho
σ	|\sigma	|τ|	\tau
υ	|\upsilon	|φ|	\phi
χ	|\chi	|ψ|	\psi
ω	|\omega|	 	

</br>
</br>

参考链接：
https://www.zybuluo.com/knight/note/96093