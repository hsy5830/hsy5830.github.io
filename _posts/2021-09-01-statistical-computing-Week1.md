---
layout: single
title:  "[Advanced Statistical Computing] Introduction"
date:   2021-09-01 16:27:00 +0530
categories: 통계계산
tags : Julia
toc: true
toc_sticky: true
toc_label: "Contents"
---

# Introduction to Julia, part1

## Types of programming languages

- compiler languages : C/C++

    기계어인 machine code로 바로 컴파일하는 방식. 빠르고, 메모리 효율이 좋지만 다소 어렵다는 단점이 있다.

- Interpreter languages : R, Python, JavaScript

    읽고 쓰기 쉽다는 게 큰 장점.

- Mixed lagnuages : Java, Kotlin

    위의 두 가지를 합친 언어. 

- Database lagnuages : SQL, Hive (Hadoop)

위 네 가지 카테고리의 언어들을 적어도 하나씩은 할 줄 알면 좋다.

프로그래밍 할 땐, loop를 최소화. → '**vectorize**' code

> The only loop you are allowed to have is that for an iterative algorithm.

## Julia? R?

Julia is a high-level, high-performance dynamic programming language for technical computing

Julia aims to:

> Walks like Python. Runs like C.

보기 쉽고, 속도도 빠른.

R은 데이터 크기가 커질수록 성능이 나빠진다. 

## Learning resources

[Julia for R Programmers](http://pages.stat.wisc.edu/~bates/JuliaForRProgrammers.pdf)

[Cheat sheet](https://juliadocs.github.io/Julia-Cheat-Sheet/)

## Basic Julia code

- Greek, Emoji 변수

```julia
# Greek letters : \latexcode + <tab>
π # 그리스 문자도 변수로 지정 가능

# Emoji도 사용가능
\:kissing_cat: + <tab>
```

- Vector & Matrix

```julia
x = zeros(5) # rep(0, 5) in R
x = zeros(Int, 5) # Int형으로 선언
x = zeros(5, 3) # 5x3 행렬

x = ones(5, 3) # 5x3 행렬 with value 1.0

# 값 선언되지 않은 행렬
x = Matrix{Float64}(undef, 5, 3)

# 빈 행렬 채우기
fill!(x, 0) # 0으로 채우기

# vector 연산
x .+ 3 # 값들에 +3
```

- 분수계산

```julia
a = 3//5 # 분수 3/5
3//5 + 3//7 # = 36//35 ... 분수 계산도 가능
```

- random value

```julia
# uniform[0,1) random value
x = rand(5,3)

# random number
x = rand(1:5, 5, 3)

# range 객체
1:10
1:2:10 # 1,3,5,7,9 ... 1에서 2칸씩 10까지

# seq in r ... 벡터생성
x = collect(1:10)
[1:10...]
```

## Julia package system

```julia
using Pkg # 'library' in R 
# import Pkg
Pkg.activate("../..") # root dirctory
Pkg.dependencies()

Pkg.add("Distribution") # 'install.pakages' in R
```

## Calling R from Julia

```julia
using RCall # 파이썬은 PyCall

x = randn(1000)
R"""
hist($x, main="I'm plotting a Julia vector")
"""
y = collect(x) # Julia 벡터로 쓰기 위함
```

## Timing and benchmark

```julia
using Random
Random.seed!(123) # set.seed in R
x = rand(1_000_000)

@time sum(x) # first run : 컴파일시간 포함
```

```julia
using BenchmarkTools
bm = @benchmark sum($x)
```