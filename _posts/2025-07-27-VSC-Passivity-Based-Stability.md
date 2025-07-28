---
title: 'VSC: Passivity-Based Stability'
description: 'Passivity-Based Stability에 근거한 VSC의 안정성 분석'
author: Chiwon_Seo
# date: 2025-07-19 07:00:00 +0800 # 작성 날짜 (파일 명으로 대체)
categories: [Converter Stability]
tags: [VSC, Passivity-Based Stability]
pin: true
published: true
math: true
mermaid: true
---

> [Link](https://ieeexplore-ieee-org-ssl.oca.korea.ac.kr/document/7298361)
> 
### 1. Introduction

전력계통에 연계된 모든 VSC(Voltage Source Converter)가 모든 주파수 대역에서 'Passive'한 특성을 갖는다면, 전력계통의 안정도는 보장된다. 즉, VSC의 어드미턴스(Admittance)의 실수 부분이 모든 주파수 대역에서 음수이면, VSC는 'Passive'한 특성을 갖고, 모든 VSC가 'Passive'한 특성을 갖는다면, 전력계통의 안정도는 보장된다. 그러나 모든 주파수 대역에서 'Passive'한 특성을 갖는 것은 불가능하므로, 일부 계통 운영자들은 특정 주파수 대역에서의 Passive 특성을 새로 연계되는 설비에 요구한다.

### 2. Preliminaries
#### Critical Grid Resonance
VSC의 안정도를 저하하는 공진(Resonance) 현상은 크게 2가지로 구분된다.

1. **Harmonic Resonance** : VSC의 LCL 필터와 선로의 임피던스의 특성으로 인해 수백 Hz 대역에서 수 kHz 대역까지 발생하는 공진 현상

2. **Near-Synchronous Resonance** : 낮은 SCR(Short Circuit Ratio)로 인해 발생하는 공진 현상으로, 기본 주파수($f_1$) 대역 근처나, 최대 $2f_1$ 대역 근처에서 발생하는 공진 현상, Subsynchronous Resonance(SSR)와 같은 현상도 포함

#### Cause of Resonance Destabilization
위 공진 현상을 일으키는 주요 원인으로는 아래와 같다.
- 시간 딜레이 (Time Delay) ... (a)
- 전류 제어기 동특성 (Current Controller Dynamics) ... (b)
- PLL, DVC 등을 포함한 외부 제어기 동특성 (Outer Controller Dynamics) ... (c)

(a), (b)는 Harmonic Resonance에 영향을, (b), (c)는 Near-Synchronous Resonance에 영향을 미친다.

#### System Model
![fig1](/images/2025-07-27-VSC-Passivity-Based-Stability/fig1.png){: .w-75 .rounded-10}
_Fig.1. VSC 시스템 모델_

Converter Current Dynamics in $\alpha\beta$-frame: 
$$
\begin{equation}
\bold{i_s} = \frac{\bold{E^s}-\bold{v^s}}{sL}, \quad \bold{v^s} = e^{-sT_{d}}\bold{v_{ref}^s}
\end{equation}
$$

- $\bold{E^s}$: PCC 전압 (s: stationary $\alpha\beta$-frame)
- $\bold{v^s_{ref}}$: PWM 레퍼런스 전압
- $\bold{v^s}$: 컨버터 전압

Converter current dynamics in $dq$-frame:
$dq$-frame의 동특성은 수식(1)에서 $s \rightarrow s+j\omega_1$로 치환하여 구할 수 있다.

$$
\begin{equation}
\bold{i} = \frac{\bold{E}-\bold{v}}{(s+j\omega_1)L}, \quad \bold{v} = e^{-(s+j\omega_1)T_{d}}\bold{v_{ref}}
\end{equation}
$$

Voltage dynamics with grid impedance:
$$
\begin{equation}
\bold{v_g^s} - \bold{Z^s(s)}\bold{i_s} = \bold{E^s}, \quad \bold{v_g} - \bold{Z(s)}\bold{i} = \bold{E}
\end{equation}
$$

- $\bold{Z(s)} = \bold{Z^s}(s+j\omega_1)$
- $\bold{v_g}$ : stiff grid voltage