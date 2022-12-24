---
emoji: 🧮
title: LEETCODE 70. Climbing Stairs
date: '2022-12-24 09:36:00'
author: Roach
tags: algorithm leetcode
categories: algorithm
---

## 문제 이해

Input 으로 주어진 높이가 나오고, 1 / 2 Step 만 밟을 수 있음.  
그때 오를 수 있는 모든 가짓수는?

## 풀이 방법 추상화(수도 코드)

Input 1 = 1 (1)  
Input 2 = 1 + 1 / 2 (2)  
Input 3 = 1 + 1 + 1 / 2 + 1 / 1 + 2 (3)  
Input 4 = 1 + 1 + 1 + 1 / 2 + 2 / 1 + 1 + 2 / 1 + 2 + 1 / 2 + 1 + 1 (5)  
Input 5 (8)  
- 1 + 1 + 1 + 1 + 1
- 1 + 2 + 2 
- 2 + 1 + 2 
- 2 + 2 + 1
- 2 + 1 + 1 + 1
- 1 + 2 + 1 + 1  
- 1 + 1 + 2 + 1
- 1 + 1 + 1 + 2

반복되는 패턴을 찾아보면 아래와 같음

$a_n = a_{n-1} + a_{n - 2}$

## 코드로 구현

```kotlin
fun climbStairs(n: Int) = if (n <= 1) 1 else climbStairs(n - 1) + climbStairs(n - 2)
```
## 최적화

근데 이렇게 스택 프레임을 유지하는것보다 꼬리 재귀를 이용하면 더 크게 최적화가 가능하다.  
꼬리 재귀 최적화를 위해서는 반복문으로 변경해야 한다.  

일반적으로 꼬리재귀로 구현하기 위해서는 반복문으로 변경한뒤 재귀로 바꿔야 좀 더 쉽게 변경이 가능하다. 

```kotlin
fun climbStairsForIter(n: Int): Int =
    if (n <= 2) n
    else {
        var acc1 = 1
        var acc2 = 2
        (3 .. n).forEach { _ ->
            val temp = acc2
            acc2 += acc1
            acc1 = temp
        }
        acc2
    }
```

반복문으로 변경하면 위와 같이 변경된다. 
이제 인자로 넘길 수를 찾아보면 acc2 = acc1 + acc2 이고, acc1 은 acc2 를 받아야 한다. 그래서 꼬리재귀함수로 최적화 해보면 아래와 같다.

```kotlin
tailrec fun climbStairs(n: Int, acc1: Int = 1, acc2: Int = 2): Int = when (n) {
    1 -> acc1
    2 -> acc2
    else -> climbStairs(n = n - 1, acc1 = acc2, acc2 = acc2 + acc1)
}
```

## 회고

- 배운점
    - 꼬리 재귀로 최적화 하려면 반복문으로 일단 바꾸는 사고를 해야 함.