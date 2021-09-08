---
layout: single
title:  "[Advanced Statistical Computing] Julia Introduction"
date:   2021-09-01 16:27:00 +0530
categories: Statcom
tags : Julia
toc: true
toc_sticky: true
toc_label: "Contents"
---
참고 : Julia Intro 2
<br>

# Matrices and Vectors
<br>

## Dimensions(차원)

```julia
x = randn(5,3)

size(x) # (5,3) : 전체 차원 수
size(x, 1) # 5 : 행의 수
size(x, 2) # 3 : 열의 수
length(x) # 15 : 전체 성분의 수
```
<br><br>

## Indexing(인덱싱)
<br>

```julia
x = randn(5,5) # 5x5 행렬

x[:, 1] # 1st column
x[1, :] # 1st row
x[1:2, 2:3] # sub-array / 1~2행, 2~3열 선택
```
파이썬과 다르게, 줄리아에선 인덱싱을 할 때 마지막 번호의 인덱스가 제외되지 않는다.
<br><br>

```julia
z = x[1:2, 2:3]

# 같은 코드
z = view(x, 1:2, 2:3)
@views z = x[1:2, 2:3]
```
위의 두 코드 모두 z에 x 부분행렬을 저장하게 되는데, 두 방법에는 차이가 있다.<br>
`z = x[1:2, 2:3]` 의 경우에는 메모리를 새롭게 할당(allocate)하여 z라는 변수에 x의 부분행렬을 저장하는 것이지만, `z = view(x, 1:2, 2:3)` 의 경우엔 같은 포인터를 가리키는 하나의 변수로서 z를 이용하게 된다.

```julia
z[2,2] = 0.0
```