---
title: 1.2 Dynamic Mode Decomposition - Algorithm
description: ""
author: Chiwon_Seo
date: 2025-11-15 00:00:02 +0900 # 작성 날짜 (파일 명으로 대체)
categories: [Data-Driven, Data-Driven Methods]
tags: [Koopman Operator, Data-Driven Methods]
pin: true
published: true
math: true
mermaid: true
---

### The DMD Algorithm
아래의 관측 데이터 (1)을 이용해 $\mathbf{A}$ 행렬을 추정하는 방법은 수식 (2)와 같다.

$$
\begin{equation}
\mathbf{X} = [\mathbf{x}_1, \mathbf{x}_2, \ldots, \mathbf{x}_{m-1}], \quad \mathbf{X'} = [\mathbf{x}_2, \mathbf{x}_3, \ldots, \mathbf{x}_m]
\end{equation}
$$

$$
\begin{equation}
\mathbf{A} \approx \mathbf{X'} \mathbf{X}^{\dagger}
\end{equation}
$$

만약 x의 차원이 매우 크다면, $\mathbf{A}$ 행렬은 'tall-skinny' 행렬이 되어 유사 역행렬의 계산 부담이 증가하게 된다. 이를 해결하기 위해 DMD 알고리즘은 SVD (Singular Value Decomposition)를 사용하여 차원 축소를 수행한다. 먼저 관측 데이터 행렬 $\mathbf{X}$에 대해 SVD를 적용한다.

$$
\begin{equation}
\mathbf{X} \approx \mathbf{U} \mathbf{\Sigma} \mathbf{V}^*
\end{equation}
$$

이때, $\mathbf{U} \in \mathbb{R}^{n \times r}$, $\mathbf{\Sigma} \in \mathbb{R}^{r \times r}$, $\mathbf{V} \in \mathbb{R}^{m \times r}$이며, $r$은 축소 차원의 크기이다. '*'은 실수 행렬에 대해서는 전치(Transpose) 연산을 의미하며, 복소수 행렬에 대해서는 켤레 전치(Conjugate Transpose) 연산을 의미한다. $r$이 $n$과 $m$보다 작을 경우, SVD의 결과는 원래 행렬 $\mathbf{X}$와 완벽히 일치하지 않으며, 근사한 값을 갖는다. SVD를 통해 도출된 행렬 $\mathbf{U}$, $\mathbf{V}$는 아래와 같은 특성을 갖는다.

$$
\begin{equation}
\mathbf{U}^* \mathbf{U} = \mathbf{I}, \quad \mathbf{V}^* \mathbf{V} = \mathbf{I}
\end{equation}
$$

만약 관측한 데이터가 복잡한 동특성을 갖지 않는다면 (데이터가 저차원 구조를 갖는다면), 특이값 (Singular Value) 행렬 $\mathbf{\Sigma}$의 대각 원소들 중 상당수가 0에 가까운 값을 갖게 된다. 일반적인 저차원 근사는 0이 아닌 Singular Value를 기준으로 $r$을 선택한다. 이러한 방식으로 저차원 근사를 수행할 경우, 데이터에 혼재된 잡음(Noise) 성분이 제거되는 효과도 얻을 수 있다.

#### 왜 SVD가 Moore-Penrose 유사 역행렬 계산에 도움이 되는가?
SVD를 사용하면, $\mathbf{X}$의 유사 역행렬을 다음과 같이 계산할 수 있다.

$$
\begin{equation}
\begin{aligned}
\mathbf{X}^{\dagger} &= (\mathbf{X}^* \mathbf{X})^{-1} \mathbf{X}^* \\
&= (\mathbf{V} \mathbf{\Sigma}^* \mathbf{U}^* \mathbf{U} \mathbf{\Sigma} \mathbf{V}^*)^{-1} \mathbf{V} \mathbf{\Sigma}^* \mathbf{U}^* \\
&= (\mathbf{V} \mathbf{\Sigma}^* \mathbf{\Sigma} \mathbf{V}^*)^{-1} \mathbf{V} \mathbf{\Sigma}^* \mathbf{U}^* \\
&= \mathbf{V} (\mathbf{\Sigma}^* \mathbf{\Sigma})^{-1} \mathbf{\Sigma}^* \mathbf{U}^* \\
&= \mathbf{V} \mathbf{\Sigma}^{-1} \mathbf{U}^* \;(\because \mathbf{\Sigma} \text{ is diagonal})
\end{aligned}
\end{equation}
$$

위 결과를 토대로, $\mathbf{A}$ 행렬의 추정치는 다음과 같이 계산된다.

$$
\begin{equation} 
\mathbf{A} \approx \mathbf{X'} \mathbf{V} \mathbf{\Sigma}^{-1} \mathbf{U}^*
\end{equation}
$$

여기서 연산 부담을 더 줄이기 위해, $\mathbf{A}$ 행렬의 저차원 표현 $\tilde{\mathbf{A}}$를 다음과 같이 정의한다.

$$
\begin{equation}
\tilde{\mathbf{A}} = \mathbf{U}^* \mathbf{A} \mathbf{U} = \mathbf{U}^* \mathbf{X'} \mathbf{V} \mathbf{\Sigma}^{-1}
\end{equation}
$$

$\tilde{\mathbf{A}}$ 행렬은 $r \times r$ 크기를 가지므로, $\mathbf{A}$ 행렬보다 훨씬 작은 크기를 갖는다. 따라서 $\tilde{\mathbf{A}}$ 행렬의 고유값과 고유벡터를 계산하는 것이 연산적으로 더 효율적이다. $\tilde{\mathbf{A}}$의 고유값과 고유벡터는 다음과 같이 정의된다

$$
\begin{equation}
\tilde{\mathbf{A}} \mathbf{W} = \mathbf{\Lambda} \mathbf{W}
\end{equation}
$$

식 (8)에서 $\mathbf{\Lambda}$는 $\tilde{\mathbf{A}}$의 고유값을 대각 성분으로 갖는 대각 행렬이고, $\mathbf{W}$는 대응하는 고유벡터를 열 벡터로 갖는 행렬이다. $\tilde{\mathbf{A}}$의 고유값과 고유벡터를 이용하여, 원래 시스템 행렬 $\mathbf{A}$의 고유벡터 $\Phi$를 다음과 같이 복원할 수 있다.

$$
\begin{equation}
\Phi = \mathbf{X'} \mathbf{V} \mathbf{\Sigma}^{-1} \mathbf{W}
\end{equation}
$$

#### 왜 $\tilde{\mathbf{A}}$의 고유값은 $\mathbf{A}$의 고유값과 동일한가?
$\tilde{\mathbf{A}}$의 고유값은 $\mathbf{A}$의 고유값과 동일하다. 이를 증명하기 위해, $\mathbf{A}$의 고유값 문제를 다음과 같이 쓸 수 있다.

$$
\begin{equation}
\mathbf{A} \mathbf{u} = \lambda \mathbf{u}
\end{equation}
$$
$$
\begin{equation}
\mathbf{U}^* \mathbf{A} \mathbf{u} = \lambda \mathbf{U}^* \mathbf{u}
\end{equation}
$$
$$
\begin{equation}
\tilde{\mathbf{A}} \mathbf{U}^* \mathbf{u} = \lambda \mathbf{U}^* \mathbf{u}
\end{equation}
$$

위 식에서, $\mathbf{U}^* \mathbf{u}$가 0이 아닌 벡터라고 가정하면, $\tilde{\mathbf{A}}$의 고유값 문제와 동일한 형태가 됨을 알 수 있다. 따라서 $\tilde{\mathbf{A}}$와 $\mathbf{A}$는 동일한 고유값을 갖는다.

#### $\tilde{\mathbf{A}}$의 고유벡터로부터 $\mathbf{A}$의 고유벡터 복원 (식 (9)의 유도)
식 (12)에서 도출된 $\tilde{\mathbf{A}}$의 고유벡터 $\mathbf{W}$를 이용하여, 원래 시스템 행렬 $\mathbf{A}$의 고유벡터 $\Phi$를 다음과 같이 복원할 수 있다.

$$
\begin{equation}
\Phi = \mathbf{X'} \mathbf{V} \mathbf{\Sigma}^{-1} \mathbf{W}
\end{equation}
$$

이는 식 (12)를 통해 유도되는 아래 결과와는 다르다.
$$
\begin{equation}
\Phi = \mathbf{U} \mathbf{W}
\end{equation}
$$

전자를 통해 계산된 고유 벡터는 exact DMD 모드이며, 후자는 projected DMD 모드이다 [1].


---
### References

[1] Tu, J. H. (2013). Dynamic mode decomposition: Theory and applications (Doctoral dissertation, Princeton University).

[2] Kutz, J. N., Brunton, S. L., Brunton, B. W., & Proctor, J. L. (2016). Dynamic mode decomposition: data-driven modeling of complex systems. Society for Industrial and Applied Mathematics.