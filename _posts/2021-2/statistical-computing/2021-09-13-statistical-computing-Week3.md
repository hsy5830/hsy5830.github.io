---
layout: single
title:  "[Advanced Statistical Computing] Computer Arithmetics"
date:   2021-09-13 14:52:00 +0530
categories: Julia
tags : Julia
toc: true
toc_sticky: true
toc_label: "Contents"
---

-  버전 확인

    ```julia
    # 버전 확인
    versioninfo()

    # 패키지 확인
    using Pkg
    Pkg.activate("../..")
    Pkg.status()
    ```

<br><br>

# Units of computer storage

&nbsp;&nbsp;사람과 컴퓨터가 숫자를 사용하는 방식에는 차이가 있다. 간단하게 말하면 사람은 10진수를, 컴퓨터는 2진수를 사용한다. 왜 그럴까?<br>
&nbsp;&nbsp;사람과 컴퓨터의 물리적 차이에 그 이유가 있다. 사람은 손가락이 10개기 때문에 10진수가 사용하기 가장 편했을 것이고, 컴퓨터는 전기가 On이냐 Off냐 두 경우에 따라 표현을 하기에 2진수가 가장 편했을 것이다.<br>

컴퓨터 저장단위의 예시

* Bit = binary digit
* byte = 8 bits
* KB = $10^3$ bytes
* MB = $10^6$ bytes
* GB = $10^9$ bytes
 
<br>

코드에서 사용하는 변수가 차지하는 메모리의 양을 알아보자.
```julia
x = rand(100, 100)
Base.summarysize(x) # x가 쓰는 메모리의 양 / 80040
```
<br>

`varinfo()` : 사용하고 있는 변수의 메모리 크기와 종류에 대해 볼 수 있다.
```julia
varinfo() 
```
```
name	size	summary
Base		Module
Core		Module
Main		Module
x	78.164 KiB	100×100 Matrix{Float64}
```
<br><br>

# Storage of Characters

Character 타입이 저장되는 방법을 알아보자.<br>


## ASCII
<Br>

* ASCII

    `American Standard Code for Information Interchange` : 7 bits, only $2^7$=127개 characters.

    ```julia
    # integers 0, 1, ..., 127 and corresponding ascii character
    [0:127 Char.(0:127)]
    ```
    ```
    128×2 Matrix{Any}:
    0  '\0'
    1  '\x01'
    2  '\x02'
    3  '\x03'
    4  '\x04'
    5  '\x05'
    6  '\x06'
    7  '\a'
    8  '\b'
    9  '\t'
    10  '\n'
    11  '\v'
    12  '\f'
    ⋮  
    116  't'
    117  'u'
    118  'v'
    119  'w'
    120  'x'
    121  'y'
    122  'z'
    123  '{'
    124  '|'
    125  '}'
    126  '~'
    127  '\x7f'
    ```
<br>

* Extended ASCII: 8bits, $2^8$=256

    ASCII 코드에 128개가 추가된 것


    ```julia
    # integers 128, 129, ..., 255 and corresponding extended ascii character
    # show(STDOUT, "text/plain", [128:255 Char.(128:255)])
    [128:255 Char.(128:255)]
    ```
    ```
    128×2 Matrix{Any}:
    128  '\u80'
    129  '\u81'
    130  '\u82'
    131  '\u83'
    132  '\u84'
    133  '\u85'
    134  '\u86'
    135  '\u87'
    136  '\u88'
    137  '\u89'
    138  '\u8a'
    139  '\u8b'
    140  '\u8c'
    ⋮  
    244  'ô'
    245  'õ'
    246  'ö'
    247  '÷'
    248  'ø'
    249  'ù'
    250  'ú'
    251  'û'
    252  'ü'
    253  'ý'
    254  'þ'
    255  'ÿ'
    ```
<br>

* Unicode

    `UTF-8`, UTF-16, UTF-32 가 있고, 그중 UTF-8이 가장 널리 쓰이고 있다. Julia는 모든 UTF-8 characters을 제공하므로 수식또한 자유롭게 쓸 수 있다. `\LaTex_symbol`형식으로 사용가능.

    ```julia
    # \beta-<tab>
    β = 0.0
    # \beta-<tab>-\hat-<tab>
    β̂ = 0.0
    ```

    Unicode 참조링크 : [https://docs.julialang.org/en/v1.1/manual/unicode-input/#Unicode-Input-1](https://docs.julialang.org/en/v1.1/manual/unicode-input/#Unicode-Input-1)

<br><br>

# Integers : fixed-point number system(고정 소수점)

&nbsp;&nbsp;정수($\mathbb{Z}$)는 무수히 많은 수를 포함하고 있지만, 컴퓨터의 메모리는 그렇지 않다. 따라서 컴퓨터는 수를 저장할 때 근사를 이용하여 최대한 많은 수를 저장한다.<br>

&nbsp;&nbsp;상수 *M*은 비트 수를 뜻하고, 부호를 나타내는 방식에는 여러가지가 있다. 예를 들어, *M*=64 라면, 최대로 처리할 수 있는 비트 수가 $2^{64}$개인 것이다.