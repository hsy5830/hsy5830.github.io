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

$$y_i = \beta_0 + \beta_1 x_{1i} + \dotsb + \beta_q x_{qi} + \beta_{q+1} x_{(q+1)i} + \dotsb + \beta_p x_{pi} + \epsilon_i, \quad \epsilon_i \stackrel{iid}{\sim} N(0,\sigma^2)$$

행렬을 이용하여 나타내면,

$$y = X\beta + \epsilon = \left(X_1 X_2\right)\left(\begin{array}{c}\beta^*_1\\ \beta^*_2\end{array}\right) + \epsilon = X_1\beta^*_1+X_2\beta^*_2 + \epsilon, \; \epsilon \sim N(0, \sigma^2I_n)$$

위 식의 $\beta$ 들은  

$$\beta^*_1 = ( \beta_0, \beta_1, \dotsb, \beta_q)$$

$$\beta^*_2 = (\beta_{q+1}, \dotsb, \beta_p)$$

이다.

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

이고 추가적으로 

$$SSR_F = SSR_{\beta^*_2|\beta^*_1} + SSR_{\beta^*_1}$$ 

임을 알 수 있다.

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

검정을 위한 `검정통계량 F`는 아래와 같이 정의 할 수 있다.

$$
\begin{align*}
    F &= \frac {MSR_{\beta^*_2|\beta^*_1}}{MSE_F} \\
    &= \frac {\frac{SSR_{\beta^*_2|\beta^*_1}}{\sigma^2}/(p-q)}{\frac{SSE_F}{\sigma^2}/(n-p-1)} \sim F_{p-q, n-p-1}
\end{align*}
$$

<br>

유의수준 $\alpha$의 검정에서, 만약 $F> F_{p-q, n-p-1}(\alpha)$ 라면, 귀무가설 $H_0$를 기각한다.

<br><br>

# LRT(Likelihood Ratio Test)

$\beta, \sigma^2$의 MLE(Maximum Likelihood Estimator)는 각각 다음과 같다.

$$\hat{\beta} = (X'X)^{-1}X'y, \;\; \hat{\sigma^2} = \frac 1n ||y-X\hat{\beta}||^2$$

또한 귀무가설 하에서, $\beta^*_1, \sigma_0^2$의 MLE는 비슷한 형식으로

$$\hat{\beta^*_1} = (X_1'X_1)^{-1}X_1'y,\;\;\hat{\sigma_0^2} = \frac 1n ||y-X_1\hat{\beta^*_1}||^2$$

이다.

가능도비(likehood, L)은 

$ \log L = (2\pi\sigma)^{-n/2}  exp(-(y-X\beta)'(y-X\beta)/2\sigma^2)$

로 표현되는데, 각각의 상황에서의 Likelihood는

$$L_1 = L(\hat{\beta},\hat{\sigma^2)}, \;\; L_0 = L(\hat{\beta^*_1}, \hat{\sigma_0^2})$$

이다.

<br>

가능도비검정을 위한 통계량은 다음과 같이 정의된다.

$$
\begin{align*}
    -2 \log \Lambda 
        &= -2 \log \frac {L_0}{L_1} \\
        &=-2\left( -\frac n2 \log \frac{\hat{\sigma_0^2}}{\hat{\sigma^2}} - \frac{1}{2\hat{\sigma_0^2}} (y-X_1\hat{\beta^*_1})'(y-X_1\hat{\beta^*_1}) + \frac{1}{2\hat{\sigma^2}} (y-X\hat{\beta})'(y-X\hat{\beta}) \right) \\
        &=-2\left( -\frac n2 \log \frac{\hat{\sigma_0^2}}{\hat{\sigma^2}} - \frac n2  + \frac n2 \right) \\
        &= n \log \frac{\hat{\sigma_0^2}}{\hat{\sigma^2}}
\end{align*}
$$

또한 이 통계량은 카이제곱분포를 따르고, 자유도는 두 모형에서 추정하는 모수의 개수의 차이와 일치한다.

$$-2 \log \Lambda \sim \chi^2_{p-q}$$

<br>

따라서 유의수준 $\alpha$의 검정에서, 만약 $-2 \log \Lambda> \chi^2_{p-q}(\alpha)$ 라면, 귀무가설 $H_0$를 기각한다.

<br><br>

# Wald


우리는 $\beta = (\beta^*_1 \; \beta^*_2)$ , $\beta^*_2 = (\beta_{q+1}, \dotsb, \beta_p)$ 에 대해서, 우리는 귀무가설 $H_0 : \beta^*_2=0$ 을 검정하려고 한다.

일반적으로 $H_0 : \beta = \beta_0$ 를 검정할 때 사용하는 Wald 통계량은

$$W = (\hat{\beta} - \beta_0)'[\widehat{cov}(\hat{\beta})]^{-1}(\hat{\beta} - \beta_0)$$

인데, 우리가 다루고 있는 문제의 상황에 적용해보면 Wald 통계량은

$$ W = \hat{\beta^*_2}'[\widehat{cov}(\hat{\beta^*_2})]^{-1}\hat{\beta^*_2} $$

이고, 구성하는 식들은 다음과 같다.

$$
\begin{align*}
    \hat{\beta^*_2} &= (\hat{\beta_{q+1}}, \dotsb, \hat{\beta_p}) \; of \; \hat{\beta} \\
    \widehat{cov}(\hat{\beta^*_2}) &= (\hat{\beta_{q+1}}, \dotsb, \hat{\beta_p}) \text{에 관한} \; Var(\hat{\beta}) \; \text{의 블록행렬}
\end{align*}
$$

<br>

이 때, $W$는 카이제곱분포를 따르고, 자유도는 $rank(\widehat{cov}(\hat{\beta^*_2}))$를 따른다. 여기서 또한 자유도를 귀무가설하에서 추정하는 모수의 수와 제약이 없는 상황에서 추정하는 모수의 수의 차이로 생각해도 문제가 없다.

$$W \sim \chi^2_{p-q}$$ 

이므로, 유의수준 $\alpha$ 검정에서 만약 $W > \chi^2_{p-q}(\alpha)$ 이면, 귀무가설 $H_0$를 기각한다.

<br><br>

# Score

우선 score funtion 을 정의하자.

$$u(\beta) = \left(\frac{\partial \log L}{\partial \beta_0}, \frac{\partial \log L}{\partial \beta_1}, \dotsb, \frac{\partial \log L}{\partial \beta_p}\right)^T = (u_1(\beta^*_1, \beta^*_2)^T, \; u_2(\beta^*_1, \beta^*_2)^T)^T$$

이고, $\beta^*_0 = (\beta_0, \beta_1, \dotsb, \beta_q, 0, \dotsb, 0) = (\beta^*_1, \underset{\hat{}}{0})$ 라고 하면, Score 통계량은 

$$S = u(\beta^*_0)'[I(\beta^*_0)]^{-1}u(\beta^*_0)$$ 

로 정의된다. 

그런데 귀무가설 하에서, $\beta^*_2$ 의 값을 알지만 $\beta^*_1$ 에 대한 정보는 갖고있지 않기때문에 위의 통계량을 그대로 사용할 수 없다. 따라서 귀무가설 하에서의 $\hat{\beta^*_1}^{MLE}$ 를 추정값으로 사용한다.

$$\hat{\beta^*_1}^{MLE} = (X_1'X_1)^{-1}X_1'y$$

그렇다면 Score 통계량은,

$$
\begin{align*}
    S &= u_2(\hat{\beta^*_1}, 0)'[cov(u_2(\hat{\beta^*_1}, 0))]^{-1}u_2(\hat{\beta^*_1}, 0) \\
        &= u_2(\hat{\beta^*_1}, 0)'[I(\beta^*_0)^{-1}]_{22}u_2(\hat{\beta^*_1}, 0)
\end{align*}
$$

로 정의된다.

그렇다면 $[I(\beta^*_0)^{-1}]$ 는 어떻게 구할까?

$$u_2(\beta^*_1,\beta^*_2) = \frac 1{\sigma^2} X_2'(y-X_1\beta^*_1-X_2\beta^*_2)$$

$$
I(\beta) = 
\begin{pmatrix}
I_{11} & I_{12} \\
I_{21} & I_{22}
\end{pmatrix} , \quad
I(\beta)^{-1} = \begin{pmatrix}
I^{11} & I^{12} \\
I^{21} & I^{22}
\end{pmatrix}
$$

$$I^{22} = (I_{22}-I_{21}I^{-1}_{11}I_{12})^{-1} = [X_2'X_2 - X_2'X_1(X_1'X_1)^{-1}X_1'X_2]^{-1}\sigma_0^2$$

임을 이용하면,

$$ S = \frac 1{\hat{\sigma_0^2}}(y-X_1\hat{\beta^*_1})'X_2[X_2'(I-H)X_2]^{-1}X_2'(y-X_1\hat{\beta^*_1})$$ 

이고,

$$S \sim \chi^2_{p-q}$$

인 분포를 갖는다.

<Br>

따라서, 유의수준 $\alpha$ 검정에서 만약 $S > \chi^2_{p-q}(\alpha)$ 이면, 귀무가설 $H_0$를 기각한다.


<br><br>

# 주의할 점 (다른 귀무가설 상황의 경우)

위에서 설명한 검정은 여러가지 조건 덕분에 과정이 편하게 진행되었다. 예를 들어,

* $\epsilon \stackrel{iid}{\sim} N(0, \sigma^2)$

    위 조건은 $y \stackrel{iid}{\sim} N(0, \sigma^2 I_n)$ 의 분포를 가정하기 때문에 계산이 훨씬 쉽다. 

    일반적인 상황인 $$y \sim N(0,\Sigma)$$ 인 경우에 대해서도 생각해 볼 필요가 있다.

<br>

* $H_0 : \beta_1 = \dotsb = \beta_p = 0$

    우리가 이용한 가설에서 $q=0$ 인 케이스이다. 위 과정과 비슷하게 적용할 수 있다.

<br>

* $H_0 : \beta_j = 0, \; j=1,2,\dotsb,p$

    귀무가설 상황을 고려하여, Full model 과 Reduced model을 생각해보면

    $$
    \begin{align*}
        (FM) : y_i &= \beta_0 + \beta_i x_{1i} + \dotsb  + \beta_p x_{pi} + \epsilon_i \\
        (RM) : y_i &= \beta_0 + \beta_i x_{1i} + \dotsb + \beta_{(j-1)} x_{(j-1)i} + \beta_{j+1} x_{(j+1)i} + \dotsb + \beta_p x_{pi} + \epsilon_i
    \end{align*}
    $$

    축소모형은 $X$ 에서 j번째 열(column)을 제거한 행렬을 design matrix로 사용하는 모형이다.


이처럼 다양한 상황에서도 비슷하게 생각해 볼 수 있다.


---

[참고] : <br>

[https://j0shuajun.github.io](https://j0shuajun.github.io)<br>
[326.520A: Applied Statistics](https://jung.snu.ac.kr/syllabus/syllabus_appstat.pdf)
