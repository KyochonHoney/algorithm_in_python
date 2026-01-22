# Day03.

## 분할정복법(Divide & Conquer)

분할정복법은 큰 문제를 그대로 해결하기 어려울 때, 작은 문제로 **분할(Divide)**하여 재귀적으로 해결하고, 그 해결된 작은 문제들의 답을 **정복(Conquer)**하여 원래 문제의 답을 얻는 알고리즘 설계 패러다임이에요.

### 1. 배열의 최댓값 찾기

`A = [3, 0, -5, 7, 2, -4, 6, 9]`

`max(A)`를 구하는 것은 `max(A[0], max(A[1:]))`처럼 재귀적으로 생각할 수 있어요.

```
max(arr):
    if len(arr) == 1:
        return arr[0]
    return max(arr[0], max(arr[1:]))
```

*   **점화식**: `T(n) = T(n-1) + C`
*   **수행 시간 (Time Complexity)**: `O(n)`

---

### 2. 거듭제곱 `a^n` 구하기

`a`를 `n`번 곱하는 `a^n` 문제를 분할정복법으로 효율적으로 풀 수 있어요.

#### 1) 일반적인 재귀 방식 (선형 시간)

가장 기본적인 재귀 호출이에요. `a^n = a * a^(n-1)`

```python
def power1(a, n):
    if n == 1: # 바닥 조건: n이 1이면 a를 반환
        return a
    return a * power1(a, n-1) # n-1번 재귀 호출
```

*   **점화식**: `T(n) = T(n-1) + C`
*   **수행 시간**: `O(n)`

#### 2) 분할정복 방식 (지수 `n`이 짝수/홀수일 때)

`n`이 짝수일 때는 `a^n = a^(n/2) * a^(n/2)` 로, `n`이 홀수일 때는 `a^n = a^(n/2) * a^(n/2) * a` (정확히는 `a^(n//2) * a^(n//2) * a`) 로 분할하여 계산하면 곱셈 횟수를 크게 줄일 수 있어요.

```python
def power2(a, n):
    if n == 0: # 바닥 조건: n이 0이면 1을 반환 (a^0 = 1)
        return 1
    if n == 1: # 바닥 조건: n이 1이면 a를 반환 (a^1 = a)
        return a
    
    if n % 2 == 0: # n이 짝수일 때
        return power2(a, n // 2) * power2(a, n // 2)
    else: # n이 홀수일 때
        return power2(a, n // 2) * power2(a, n // 2) * a
```

*   **점화식**: `T(n) = 2 * T(n/2) + C` (n이 짝수일 때)
*   **수행 시간**: `O(n)` (아직 `O(n)`인데, 아래 `power3`처럼 최적화할 수 있어요!)

#### 3) 분할정복 방식 최적화 (중간 결과 저장)

`power2` 함수에서 `power2(a, n // 2)`를 두 번 호출하는 대신, 한 번만 호출해서 그 결과를 저장 (`x`)하여 재활용하면 중복 계산을 줄일 수 있어요.

```python
def power3(a, n):
    if n == 0: # 바닥 조건: n이 0이면 1을 반환
        return 1
    if n == 1: # 바닥 조건: n이 1이면 a를 반환
        return a
        
    x = power3(a, n // 2) # a^(n//2)를 한 번만 계산하여 저장

    if n % 2 == 0: # n이 짝수일 때
        return x * x
    else: # n이 홀수일 때
        return x * x * a
```

*   **점화식**: `T(n) = T(n/2) + C`
*   **수행 시간 (Time Complexity)**: `O(log n)` (곱셈 횟수를 n번에서 log n번으로 대폭 줄였어요!)

---