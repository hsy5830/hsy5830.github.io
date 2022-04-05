---
published: true
layout: single
title:  "Permutation Test(순열 검정법)"
date:   2022-04-05 15:46:13 +0530
categories: Statistics
tags : [Statistical test]
toc: true
toc_sticky: true
toc_label: "Contents"
---

<br>

통계학에서 가설 검정은 결국엔 어떤 집단의 분포를 파악하기 위해 이루어진다. 예를 들어 "어떤 두 분포간의 차이가 존재 하는가?" 라는 질문에 대해 일반적으로 t-test를 사용하곤 한다. 하지만 모수적(parametric) 방법인 t-test를 위해선 `정상성(Normality)`, `독립성(Independence)`, `등분산성(Homogeneity)` 등의 가정 만족해야 한다는 조건이 필요하다. 

**Permutation test(순열 검정법)**는 특별한 조건 없이 사용할 수 있는 비모수적(nonparametric)인 가설 검정 방법이다. 여러 조건들이 들어 맞을 때에는 당연히 모수적 방법이 좀 더 정확하겠지만, permutation test는 데이터의 성질에 크게 의존하지 않고 사용할 수 있다는 장점이 있다.

가설 검정의 큰 틀은 크게 다르지 않다. 귀무가설 하에서, 내가 가지고 있는 데이터로 만든 통계량의 값이 얼마나 나오지 힘든 경우인지 확인해서 정말 발생하기 힘든(유의수준 이하의 확률) 사건이라면 귀무가설을 기각하는 흐름은 비슷하다. 검정을 위해 필요한 p-value의 계산을 permutation test에선 어떻게 하는지 알아보자.

<br>

# Permutation with two samples

![](/assets/images/2022-04-05-permutation-test/samples.png)

두 샘플의 값들에 차이가 존재하는지에 대해 알아보고 싶다고 하자. 우선 비교의 기준을 설정해야 하는데, 두 집단의 표본 평균을 비교하는 것으로 생각하자. 위 그림을 예시로 들면 11개의 A, 9개의 B값에 대한 표본 평균을 계산할 수 있고, 두 표본 평균의 차이인 D를 우리의 검정통계량으로 사용한다.
$$D = \overline{X}_A - \overline{X}_B$$

이제 우리의 검정통계량 값인 $D$가 귀무가설 하에서, 얼마나 나오기 힘든 극단적인 값인지 판단해야한다. t-test같은 모수적 방법에선 검정통계량이 귀무가설에서 가정된 어떤 분포 안에서 얼마나 끝쪽에 위치하는지를 확인해서 p-value로 기각 여부를 판단한다. 

Permutation test에선 이 p-value 계산 방식에 차이가 있다. 귀무가설을 따른다면 두 집단은 동일한 분포를 따른다. 따라서 나눠진 A,B에 상관없이 두 집단이 섞이더라도 표본 평균에는 큰 차이가 없어야 할 것이다.