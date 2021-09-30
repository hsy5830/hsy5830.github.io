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

또한, 아무런 제약이 없는 상황에서 고려할 수 있는 전체모형(Full model, FM)과, 귀무가설 하에서 $\beta_{q+1} = \dotsb = \beta_p = 0$ 이 되므로, 이 때의 축소모형(Reduced model, RM)을 생각할 수 있다.

<br><br>

# F-test

설명을 위해 $J = \underset{\tilde{}}{1}\underset{\tilde{}}{1}', \;\;H = X(X'X)^{-1}X', \;\; H_1 = X_1(X_1'X_1)^{-1}X_1'$ 을 정의한다.

Reduced Model으로 부터, $SST, SSE_R, SSR_R$의 제곱합들을 계산할 수 있다.
$$
\begin{align*}
    SST &= y'\left(I-\frac Jn\right)y \\
        &= y'\left(I-H_1\right)y + y'\left(H_1-\frac Jn\right)y \\
        &= SSE_R + SSR_R
\end{align*}
$$

마찬가지로 Full Model로 부터,
$$
\begin{align*}
    SST &= y'\left(I-\frac Jn\right)y \\
        &= y'\left(I-H\right)y + y'\left(H-\frac Jn\right)y \\
        &= y'\left(I-H\right)y + y'\left(H-H_1\right)y + y'\left(H_1-\frac Jn\right)y \\
        &= SSE_F + SSR_{\beta^*_2|\beta^*_1} + SSR_{\beta^*_1}
\end{align*}
$$

이고 추가적으로 $SSR_F = SSR_{\beta^*_2|\beta^*_1} + SSR_{\beta^*_1}$ 임을 알 수 있다.

<br>

귀무가설 하에서, 

$$
\begin{align*}
    \frac{SSR_{\beta^*_2|\beta^*_1}}{\sigma^2} &\sim \chi^2_{p-q} = \chi^2_{rank(H-H_1)}\\
    \\
    \frac{SSE_F}{\sigma^2} \; \; &\sim \chi^2_{n-p-1} = \chi^2_{rank(I-H)}
\end{align*}
$$

임을 알고있고, 두 통계량은 서로 독립이다. (두 통계량의 공분산을 계산해보면 알 수 있다.)

<br>

검정을 위한 검정통계량 `F`는 아래와 같이 정의 할 수 있다.

$$
\begin{align*}
    F &= \frac {MSR_{\beta^*_2|\beta^*_1}}{MSE_F} \\
    &= \frac {\frac{SSR_{\beta^*_2|\beta^*_1}}{\sigma^2}/(p-q)}{\frac{SSE_F}{\sigma^2}/(n-p-1)} \sim F_{p-q, n-p-1}
\end{align*}
$$

유의수준 $\alpha$의 검정에서, 만약 $F> F_{p-q, n-p-1}(\alpha)$ 라면, 귀무가설 $H_0$를 기각한다.

<br><br>

# LRT(Likelihood Ratio Test)

$\beta, \sigma^2$의 MLE(Maximum Likelihood Estimator)는 각각 다음과 같다.

$$\hat{\beta} = (X'X)^{-1}X'y, \;\; \hat{\sigma^2} = \frac 1n ||y-X\hat{\beta}||^2$$

또한 귀무가설 하에서, $\beta^*_1, \sigma_0^2$의 MLE는 비슷한 형식으로

$$\hat{\beta^*_1} = (X_1'X_1)^{-1}X_1'y,\;\;\hat{\sigma_0^2} = \frac 1n ||y-X_1\hat{\beta^*_1}||^2$$

이다.

가능도비(likehood, L)은 

$$L(\beta, \sigma^2) = (2\pi\sigma)^{-n/2}  exp(-(y-X\beta)'(y-X\beta)/2\sigma^2)$$

로 표현되는데, 각각의 상황에서의 Likelihood는

$$L_1 = L(\hat{\beta},\hat{\sigma^2)}, \;\; L_0 = L(\hat{\beta^*_1}, \hat{\sigma_0^2})$$

이다.

<br>

가능도비검정을 위한 통계량 $\Chi$는 다음과 같이 정의된다.

$$
\begin{align*}
    -2 \log \Lambda &= -2 \log \frac {L_0}{L_1} \\
        &=-2\left( -\frac n2 \log \frac{\hat{\sigma^2}}{\hat{\sigma_0^2}} - \frac{1}{2\hat{\sigma_0^2}} (y-X_1\hat{\beta^*_1})'(y-X_1\hat{\beta^*_1}) + \frac{1}{2\hat{\sigma^2}} (y-X\hat{\beta})'(y-X\hat{\beta}) \right) \\
        &=-2\left( -\frac n2 \log \frac{\hat{\sigma^2}}{\hat{\sigma_0^2}} - \frac n2  + \frac n2 \right) \\
        &= n \log \frac{\hat{\sigma_0^2}}{\hat{\sigma^2}}
\end{align*}
$$


