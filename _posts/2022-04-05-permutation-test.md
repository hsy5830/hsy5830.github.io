---
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

**Permutation test(순열 검정법)**는 특별한 조건 없이 사용할 수 있는 비모수적(nonparametric)인 가설 검정 방법이다.
