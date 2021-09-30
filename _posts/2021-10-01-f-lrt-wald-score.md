---
layout: single
title:  "회귀분석 계수에 대한 검정(F-test, LRT, Wald, Score)"
date:   2021-10-01 00:54:00 +0530
categories: Statistics
tags : [Statistical test]
toc: true
toc_sticky: true
toc_label: "Contents"
---

<br>

# 모델 설명

다음과 같은 다중 회귀 모형을 생각해보자.

$$y_i = \beta_0 + \beta_1 x_{1i} + \dotsb + \beta_q x_{qi} + \beta_{q+1} x_{(q+1)i} + \dotsb + \beta_p x_{pi} + \epsilon_i, \quad \epsilon_i \sim N(0,\sigma^2)$$

이 모형은 행렬을 이용하면 다른 형식으로 나타낼 수 있다.

$$y = X\beta + \epsilon = \left(X_1 X_2\right)\left(\begin{array}{c}\beta^*_1\\ \beta^*_2\end{array}\right) + \epsilon = X_1\beta^*_1+X_2\beta^*_2 + \epsilon, \; \epsilon \sim N(0, \sigma^2I_n)$$

<br>

이 모형의 회귀 계수 일부에 대한 검정을 해보자. 그렇다면 귀무가설은 다음과 같이 설정 할 수 있다.
$$H_0 : \beta_{q+1} = \dotsb = \beta_p = 0$$

<br><br>

# F-test

아무런 제약이 없는 상황에서 고려할 수 있는 전체모형(Full model, FM)과, 귀무가설 하에서 $\beta_{q+1} = \dotsb = \beta_p = 0$ 이 되므로, 이 때의 축소모형(Reduced model, RM)을 생각할 수 있다.

$$
\begin{align*}
    &(FM) \quad y = X_1\beta^*_1+X_2\beta^*_2 + \epsilon \\
    &(RM) \quad y = X_1\beta^*_1 + \epsilon
\end{align*}
$$

$$(FM) \quad y = X_1\beta^*_1+X_2\beta^*_2 + \epsilon$$
$$(RM) \quad y = X_1\beta^*_1 + \epsilon$$