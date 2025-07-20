---
title: 프로니 해석 (Prony Analysis)
description: 전력계통의 소신호를 분석하기 위해선 발전기, 송전선로, 재생에너지, 부하 등에 대한 정확한 모델 정보가 필요하다. 프로니 해석은 위 설비들에 대한 정확한 모델 없이도 현재 전력계통이 갖고 있는 모드(mode)에 대한 해석을 가능하게 한다. 이 포스터에서는 프로니 해석에 대한 원리와 전력계통에 적용에 대한 간단한 사례를 다룬다. 
author: Chiwon_Seo
date: 2025-07-19 06:00:00 +0800
categories: [Power System, Data Driven]
tags: [Prony Analysis]
pin: true
math: true
---

## 소신호 안정도 (Small Signal Stability)

전력계통의 모델은 전력 수급 방정식, d-q 커플링 방정식 등으로 인해 비선형적인 특성을 갖는다. 일반적인 전력계통의 소신호 안정도 해석은 특정한 운전점에서 계통을 선형화하여 고유값(eigenvalue)을 구하는 방식으로 진행된다. 이때, 계통의 고유값은 계통의 모드(mode)를 나타내며, 모드의 감쇠(damping, $\sigma_i$)와 주파수(frequency, $\omega_i$)는 고유값의 실수부와 허수부로부터 계산된다.

$$
\begin{align}
  \dot{\mathbf{x}}(t) &= A \mathbf{x}(t) + B u(t) \\
  \mathbf{y}(t) &= C \mathbf{x}(t) + D u(t)
\end{align}
$$


$$
\begin{align}
  \lambda_i &= \sigma_i + j\omega_i \quad(i=1,2,...,n) \\
\end{align}
$$

![Prony analysis of a time-domain signal](/images/2025-07-20-Prony-Analysis/image.png)

## Reference 
[1] https://en.wikipedia.org/wiki/Prony%27s_method

[2] Almunif, Anas, Lingling Fan, and Zhixin Miao. "A tutorial on data‐driven eigenvalue identification: Prony analysis, matrix pencil, and eigensystem realization algorithm." International Transactions on Electrical Energy Systems 30.4 (2020): e12283.
