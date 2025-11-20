---
title: 1.1 Dynamic Mode Decomposition - Concept
description: ""
author: Chiwon_Seo
date: 2025-11-15 00:00:01 +0900 # 작성 날짜 (파일 명으로 대체)
categories: [Data-Driven, Data-Driven Methods]
tags: [Koopman Operator, Data-Driven Methods]
pin: true
published: true
math: true
mermaid: true
---

### Dynamic Mode Decomposition (DMD)
DMD에 대해 소개하기 전에, 아래와 같은 동적 시스템을 고려해보자.

$$
\begin{equation}
\frac{d\mathbf{x}}{dt} = \mathbf{f}(\mathbf{x},t,\mu)
\end{equation}
$$

$\mathbf{x} \in \mathbb{R}^n$는 시스템의 상태 벡터(state vector)이고, $\mu$는 시스템의 파라미터를 나타낸다. 위 시스템은 연속시간 동적 시스템(continuous-time dynamical system)이다. 만약 이 시스템을 이산시간 동적 시스템(discrete-time dynamical system)으로 표현하면 다음과 같이 쓸 수 있다.

$$
\begin{equation}
\mathbf{x}_{k+1} = \mathbf{F}(\mathbf{x}_k)
\end{equation}
$$

일반적인 시스템 이론에서 상태는 시스템 혹은 제어기 내부에 감춰져 있는 변수이다. 실제로 시스템을 통해 우리가 관측하는 것은 상태가 아닌 출력(output) 이다. 시스템으로부터의 관측치를 $\mathbf{y}$ 라고 할 때, 관측치는 상태에 대한 함수로 표현할 수 있다.

$$
\begin{equation}
\mathbf{y}_k = \mathbf{g}(\mathbf{x}_k)
\end{equation}
$$

DMD는 위와 같이 정의한 동적 시스템에서의 관측치를 상태로 취급하여, 현재의 상태와 $\Delta t$ 후의 상태 사이의 선형 관계를 측정 데이터로부터 추정하는 방법이다. 즉, DMD는 다음과 같은 선형 시스템을 가정한다.

$$
\begin{equation}
\mathbf{x}_{k+1} = \mathbf{A} \mathbf{x}_k
\end{equation}
$$

위 이산 선형 시스템에 대하여, $\mathbf{A}$ 행렬의 고유값(eigenvalue)과 고유벡터(eigenvector)를 계산하면, 시스템의 자연 응답 (honogeneous response)을 다음과 같이 표현할 수 있다.

$$
\begin{equation}
\mathbf{x}_k = \sum_{j=1}^{r} \phi_j \lambda_j^k b_j = \Phi \Lambda^k b
\end{equation}
$$

$\phi_j$와 $\lambda_j$는 각각 $\mathbf{A}$의 $j$번째 고유벡터와 고유값을 나타내고, $b_j$는 초기 상태 $\mathbf{x}_0$에 대한 계수(coefficient)이다. 행렬 $\Phi$는 모든 고유벡터를 열 벡터로 가지며, $\Lambda$는 대각 행렬(diagonal matrix)로서 모든 고유값을 대각 성분으로 가진다. DMD 알고리즘은 식 (5)에 대하여 낮은 차원 근사(low-rank approximation)를 수행하여, 시스템의 주요 동적 특성을 추출한다. 이 때 $\mathbf{A}$ 행렬은 다음 식을 최소화하는 방식으로 추정된다.

$$
\begin{equation}
\min_{\mathbf{A}} || \mathbf{x_{k+1}} - \mathbf{A} \mathbf{x_k} ||_2
\end{equation}
$$

실제로 DMD는 k와 k+1 시점의 관측 데이터 외에도 다수의 시점에서의 관측 데이터를 사용하여 $\mathbf{A}$ 행렬을 추정한다. 이를 위해, $m$개의 시점에서의 관측 데이터를 다음과 같이 행렬 형태로 구성한다.

$$
\begin{equation}
\begin{aligned}
\mathbf{X} &= [\mathbf{x}_1, \mathbf{x}_2, \ldots, \mathbf{x}_{m-1}] \\
\mathbf{X'}&= [\mathbf{x}_2, \mathbf{x}_3, \ldots, \mathbf{x}_m]
\end{aligned}
\end{equation}
$$

위 행렬을 구성하는 데이터는 모두 측정치로부터 얻어진다. 이에 따라 A의 추정치는 다음과 같이 계산된다.

$$
\begin{equation}
\mathbf{A} = \mathbf{X'} \mathbf{X}^{\dagger}
\end{equation}
$$

$\dagger$는 무어-펜로즈 유사역행렬(Moore-Penrose pseudoinverse)을 나타낸다. 유사역행렬을 통해 계산된 A는 다음을 최소화 한다.

$$
\begin{equation}
|| \mathbf{X'} - \mathbf{A} \mathbf{X} ||_F
\end{equation}
$$

$F$는 프로베니우스 놈(Frobenius norm)을 나타내며 행렬 $\mathbf{X}$에 대하여 다음과 같이 계산된다.

$$
\begin{equation}
|| \mathbf{X} ||_F = \sqrt{ \sum_{i=1}^{n} \sum_{j=1}^{m} X_{ij}^2 }
\end{equation}
$$

---
### Definition of DMD (Tu et al. 2014 [1])
Suppose we have a dynamical system (1) and two sets of data,

$$
\begin{equation}
\mathbf{X} = [\mathbf{x}_1, \mathbf{x}_2, \ldots, \mathbf{x}_{m-1}] , \quad \mathbf{X'}= [\mathbf{x}_2, \mathbf{x}_3, \ldots, \mathbf{x}_m]
\end{equation}
$$

so that $\mathbf{x}_{k+1} = \mathbf{F}(\mathbf{x}_k)$, where $mathbf{F}$ is the map in (2) for time $\Delta t$. DMD computes the leading eigendecomposition of the best-fit linear operator $\mathbf{A}$ that relates the data $\mathbf{X'} \approx \mathbf{A} \mathbf{X}$:

$$
\begin{equation}
\mathbf{A} = \mathbf{X'} \mathbf{X}^{\dagger}
\end{equation}
$$

The DMD modes, also called dynamic modes, are the eigenvectors of $\mathbf{A}$, and each DMD mode corresponds to a particular eigenvalue of $\mathbf{A}$.

---
### References

[1] Tu, J. H. (2013). Dynamic mode decomposition: Theory and applications (Doctoral dissertation, Princeton University).

[2] Kutz, J. N., Brunton, S. L., Brunton, B. W., & Proctor, J. L. (2016). Dynamic mode decomposition: data-driven modeling of complex systems. Society for Industrial and Applied Mathematics.