---
layout:     post
title:      Machine Learning Homework 2
subtitle:   CS405
date:       2018-11-05
author:     Arthur Kwok
header-img: img/post-bg-miui6.jpg
header-style: text
catalog: true
tags:
    - MachineLearning
---



# TOC

## 2.2
normalized:
$\sum_x p(x\|\mu)=p(x=1\|\mu)+p(x=-1\|\mu)=\frac{1+\mu}{2}+\frac{1-\mu}{2} = 1.$

mean:
$E(x)=\sum_x xp(x\|\mu)=\frac{1+\mu}{2}-\frac{1-\mu}{2}=\mu$.

variance:
$E(x^2)=\frac{1+\mu}{2}+\frac{1-\mu}{2}=1$.

$var(x) = E(x^2)-E(x)^2=1-\mu^2$.

entropy:

$H(x) = -\sum_x p(x \| \mu)ln(x \| \mu) = -(\frac{1+\mu}{2} ln(\frac{1+\mu}{2})+\frac{1-\mu}{2} ln(\frac{1-\mu}{2}))$

---

## 2.3

$C_N^m = \frac{N!}{m!(N-m)!}$
$C_N^{m-1} = \frac{N!}{(m-1)!(N-m+1)!}$

$C_{N+1}^m = \frac{(N+1)!}{m!(N+1-m)!}=\frac{(N+1-m)!+mN!}{m!(N+1-m)!}$

$= \frac{N!}{m!(N-m)!}+\frac{N!}{(m-1)!(N-m+1)!} = C_N^{m-1}+C_N^{m}.$

---

## 2.6
$Beta(\mu\|a,b)=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}\mu^{a-1}(1-\mu)^{b-1}$

Mean:

$E(\mu)=\int_0^1\mu p(\mu)du=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}\int_0^1\mu^{(a+1)-1}(1-\mu)^{b-1}d\mu$

according to (2.265,

$=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)} \cdot \frac{\Gamma(a+1)\Gamma(b)}{\Gamma(a+1+b)}$

use the property of Gamma distribution, $\Gamma(a+1)=a\Gamma(a)$,

$=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)} \cdot \frac{a\Gamma(a)\Gamma(b)}{(a+b)\Gamma(a+b)}=\frac{a}{a+b}.$

Variance:

$E(\mu^2)=\int_0^1\mu^2 p(\mu)du=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}\int_0^1\mu^{a+2-1}(1-\mu)^{b-1}d\mu$

$=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)} \cdot \frac{\Gamma(a+2)\Gamma(b)}{\Gamma(a+b+2)}=\frac{a(a+1)}{(a+b)(a+b+1)}$.

$var(\mu)=E(\mu^2)-E(\mu)^2=\frac{a(a+1)}{(a+b)(a+b+1)}-\frac{a^2}{(a+b)^2}$

$=\frac{ab}{(a+b)^2(a+b+1)}$.

mode:

---

## 2.10

$Dir(\mu\|\alpha)=\frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\dots\Gamma(\alpha_K)}\prod_k^K\mu_k^{\alpha_k-1}$

Mean:

$E(\mu_j)=\int\mu_jp(\mu\|\alpha)d\mu = \frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\dots\Gamma(\alpha_K)}\int\mu_j\prod_k^K\mu_k^{\alpha_k-1}d\mu$

$=\frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\dots\Gamma(\alpha_K)}\frac{\Gamma(\alpha_1)\dots\Gamma(\alpha_j+1)\dots\Gamma(\alpha_K)}{\alpha_0+1}=\frac{\alpha_j}{\alpha_0}$.

Var:

$E(\mu_j^2)=\int\mu_j^2p(\mu\|\alpha)d\mu = \frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\dots\Gamma(\alpha_K)}\int\mu_j^2\prod_k^K\mu_k^{\alpha_k-1}d\mu$

$=\frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\dots\Gamma(\alpha_K)}\frac{\Gamma(\alpha_1)\dots\Gamma(\alpha_j+2)\dots\Gamma(\alpha_K)}{\alpha_0+2}=\frac{\alpha_j(\alpha_j+1)}{\alpha_0(\alpha_0+1)}$.

$var(\mu_j)=E(\mu_j^2)-E(\mu_j)^2=\frac{\alpha_j(\alpha_j+1)}{\alpha_0(\alpha_0+1)}-\frac{\alpha_j^2}{\alpha_0^2}$

$=\frac{\alpha_j(\alpha_0-\alpha_j)}{\alpha_0^2(\alpha_0+1)}$.

Cov:

$E(\mu_j\mu_l)=\frac{\alpha_j\alpha_l}{\alpha_0(\alpha_0+1)}$.

$Cov(\mu_j\mu_l)=E(\mu_j\mu_l)-E(\mu_j)E(\mu_l)=\frac{\alpha_j\alpha_l}{\alpha_0(\alpha_0+1)}-\frac{\alpha_j\alpha_l}{\alpha_0^2}$

$=-\frac{\alpha_j\alpha_l}{\alpha_0^2(\alpha_0+1)}$.

---

## 2.42
$Gam(\lambda\|a,b)=\frac1{\Gamma(a)}b^a\lambda^{a-1}exp(-b\lambda)$.

mean:

$E(\lambda)=\int_0^\infty\lambda p(\lambda)d\lambda$

$=\frac1{\Gamma(a)}\int_0^\infty b^a\lambda^aexp(-b\lambda)d\lambda=\frac1{\Gamma(a)}\int_0^\infty (b\lambda)^{(a+1)-1}exp(-b\lambda)d\lambda$

according to the definition of $\Gamma(x)$, the equation equals to:

$\frac{\Gamma(a+1)}{b\Gamma(a)} = \frac ab.$

Variance:

$E(\lambda^2) = \int_0^\infty \lambda^2p(\lambda)d\lambda$

$=\frac1{\Gamma(a)}\int_0^\infty b^a\lambda^{a+1}exp(-b\lambda)d\lambda=\frac1{\Gamma(a)}b^{-1}\int_0^\infty (b\lambda)^{(a+2)-1}exp(-b\lambda)d\lambda$

$=\frac{\Gamma(a+2)}{b^2\Gamma(a)}=\frac{a(a+1)}{b^2}$.

$Var(\lambda)=E(\lambda^2)-E(\lambda)^2=\frac{a}{b^2}.$

---

## 3.2

$\Phi(\Phi^T\Phi)^{-1}\Phi^T\times v = \Phi\times(\Phi^T\Phi)^{-1}\Phi^Tv$

This means the matrix takes any vector v and projects it onto the space spanned by the columns of $\Phi$.

Then, in tthe regression model,

$y=\Phi w_{ML}=\Phi(\Phi^T\Phi)^{-1}\Phi^T\times t$

similarly, for every column $\phi_i$ in$\phi$, we have

$(y-t)^T\phi_i=(\Phi w_{ML}-t)^T\phi_i=t^T(\Phi(\Phi^T\Phi)^{-1}\Phi^T-I)^T\phi_i = 0.$

SO, $(y-t)$ is orthogonal to $\phi$ and also orthogonal to $S$.

---

## 3.3

define $R=diag\{r_1, \dots , r_n\}$, the error could be written in the form:

$E_D(w)=\frac12(t-\Phi w)^T R(t-\Phi w)$.

Letting $\frac{dE_D(w)}{dw}=0$, we get:

$w^*=(\Phi^TR\Phi)^{-1}\Phi^TRt$.

if $r_i=1$ for all $i$, the solution is the standard circumstance;
if $r_i$ varies, it controls the precision of the error, which is a data dependent noise variance;
for replicated data points, the correspoding $r_i$ and $r_j$ adds up to work as a weighted coefficient.

---

## 3.8
likelihood:
$p(t_{N+1}\|x_{N+1}, w)=\frac{\beta}{2\pi}^{\frac 12}exp(-\frac\beta 2(t_{N+1}-w^T\phi_{N+1})^2)$

prior:
$p(w)=G(w\|m_N, SN)$

posterior:

$p(w\|t_{N+1},x_{N+1},m_N, S_N)=exp(-\frac 12(w-m_N)^TS_N^{-1}(w-m_N)-\frac12\beta(t_{N+1}-w^T\phi_{N+1})^2)$

IF we define 
$S_{N+1}^{-1}=S_{N}^{-1}+\beta\phi_{N+1}\phi_{N+1}^T$ , and 
$m_{N+1}=S_{N+1}(S_N^{-1}m_N+\beta\phi_{N+1}t_{N+1}),$

then the posetior equals to:

$G(w\|m_{N+1}, S_{N+1})$.

---

## 3.12

---

## 3.15

$E(m_N)=\frac\beta 2 \|\|t-\Phi m_N\|\|^2+\frac\alpha 2m_N^Tm_N=\frac{N-\gamma}{2}+\frac{\gamma}{2}=\frac N2.$

---

## 3.19

---

## 3.23
$p(t)=\iint p(t\|w,\beta)    p(w\|\beta)dwp(\beta)d\beta$

$=\frac{b_0^{a_0}}{\sqrt{(2\pi)^{M+N}\|S_0\|}}\iint exp(-\frac\beta 2(w-m_N)^TS_N^{-1}(w-m_N)dw$
$exp(-\frac\beta 2(t^Tt+m_0^TS_0^{-1}m_0-m_N^TS_N^{-1}m_N)))$
$\beta^{\alpha_N-1}\beta^{\frac M2}exp(-b_0\beta)d\beta$

$=\frac{b_0^{a_0}}{\sqrt{(2\pi)^{M+N}\|S_0\|}}(2\pi)^{\frac M2}\|S_N\|^{\frac12}\int\beta^{a_N-1}exp(-b_N\beta)d\beta$

$=\frac{1}{(2\pi)^{\frac N2}}\frac{\sqrt{\|S_N\|}b_0^{a_0}\Gamma(a_N)}{\sqrt{\|S_0\|}b_N^{a_N}\Gamma(a_0)}$.
