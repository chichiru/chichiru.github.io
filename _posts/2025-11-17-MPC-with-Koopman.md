---
title: MPC with Koopman Operator
description: ""
author: Chiwon_Seo
# date: 2025-07-19 07:00:00 +0800 # 작성 날짜 (파일 명으로 대체)
categories: [Data-Driven, Papers]
tags: [Koopman Operator, Data-Driven Methods]
pin: true
published: true
math: true
mermaid: true
---

### 1. Linear Predictors
아래의 이산 비선형 동적 시스템에 대하여 생각해보자

$$
\begin{equation}
x^{+} = f(x, u)
\end{equation}
$$

위 식에서 $x \in \mathbb{R}^n$는 시스템의 상태 벡터를, $u \in \mathbb{R}^m$는 제어 입력 벡터를 나타낸다. $f:\mathbb{R}^n \times \mathbb{R}^m \rightarrow \mathbb{R}^n$는 시스템의 비선형 동적 함수를 나타낸다.

Model Predictive Control (MPC)의 핵심은 주어진 초기 상태 $x_0$에서 시작하여, 미래의 제어 입력 시퀀스 $\{u_0, u_1, \ldots \}$에 대하여 상태의 궤적 (trajectory)을 예측하는 것이다. 상태를 예측하는 Predictor는 아래와 같은 선형 시스템 형태로 표현될 수 있다.

$$
\begin{equation}
\begin{aligned}
z^{+} &= A z + B u \\
\hat{x} &= C z
\end{aligned}
\end{equation}
$$

일반적으로 $z \in \mathbb{R}^N$는 이며, $N > n$ 이다. $z$의 초기치인 $z_0$는 다음과 같다.

$$
\begin{equation}
z_0 = \psi(x_0) = \begin{bmatrix}\psi_1(x_0) \\ \psi_2(x_0) \\ \vdots \\ \psi_N(x_0) \end{bmatrix}
\end{equation}
$$

위 식에서 $\psi_i$를 lifting function, $z$는 lifted state 라고 불린다.

MPC의 핵심 아이디어는 $\hat{x}$가 실제 상태 $x$와 근사하다면, predictor를 통해 설계한 최적 제어기가 실제 시스템에 대해서도 좋은 성능을 낼 수 있다는 것이다. 실제로 모든 시간에 대하여, 선형 예측기가 비선형 시스템의 상태를 정확하게 예측하는 것은 불가능하다. 그러나 충분한 시간 동안 예측 오차가 작다면, MPC는 좋은 성능을 낼 수 있다.

![Fig1](/images/2025-11-17-MPC-with-Koopman/fig1.png){: .w-75 .rounded-10}
_Fig.1. Linear predictor for a nonlinear controlled dynamical system_

---
### 2. Koopman operator - rationale behind the approach
입력이 없는 비선형 동적 시스템에 대하여, Koopman operator는 상태의 시간 진화를 관측 공간에서 선형 연산으로 표현하는 선형 연산자이다.

$$
\begin{equation}
(\mathcal{K}\psi)(x) = \psi(f(x))
\end{equation}
$$

#### 2.1 Koopman operator for controlled systems
Koopman operator에 제어 입력을 포함시키기 위해 다음과 같은 확장된 상태 (extended state)를 정의한다.

$$
\begin{equation}
\chi = \begin{bmatrix} x \\ u \end{bmatrix}
\end{equation}
$$

위 식에서 $u= (u_i)_{i=0}^{\infty}$는 입력에 대한 수열(sequence)이다. 확장된 상태에 대한 동특성은 다음과 같다.

$$
\begin{equation}
\chi^{+} = F(\chi) = \begin{bmatrix} f(x, u_0) \\ \mathcal{S}u \end{bmatrix}
\end{equation}
$$

여기서 $\mathcal{S}$는 시프트 연산자(shift operator)로, $\mathcal{S}u = (u_{i+1})_{i=0}^{\infty}$이다. 확장된 상태에 대한 Koopman operator는 다음과 같이 정의된다. 위 시스템에 대하여 Koopman Operator는 다음과 같이 정의된다.

$$
\begin{equation}
(\mathcal{K}\phi)(\chi) = \phi(F(\chi))
\end{equation}
$$

#### 2.2 EDMD for controlled systems
본 논문에서는 Koopman Operator에 대하여 Extended Dynamic Mode Decomposition (EDMD) 기법을 사용하여 유한 차원 근사를 구한다. 수식 (2)의 선형 예측기는 EDMD를 통해 구한 Koopman Operator의 근사 행렬을 사용하여 구성된다. EDMD 기법은 관측치 집합 $ \(\chi_j, \chi_j^+ \)에 대해 다음을 최소화한다.

$$
\begin{equation}
\min_{j=1}^{K}\sum_{j=1}^{M}\|\phi(\chi_j^+) - A \phi(\chi_j) \|^2_2
\end{equation}
$$

수식 (2)의 선형 예측기와, 수식 (8)을 다루기 위해 lifting function $\phi$는 다음과 같은 형태임을 가정하자.

$$
\begin{equation}
\phi_i(x,u) = \psi_i(x) + \mathcal{L}_i u
\end{equation}
$$

위 식에서 $\psi_i$는 비선형 함수이지만, $\mathcal{L}_i$는 선형 함수이다.
