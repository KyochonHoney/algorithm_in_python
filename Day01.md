# Day 01: 재귀 (Recursion)

## 재귀 (Recursion)의 이해

재귀는 함수 내부에서 자기 자신을 한 번 이상 호출하는 프로그래밍 기법을 말해요. 마치 거울이 거울을 비추는 것처럼, 더 작은 문제로 나누어 동일한 함수를 반복적으로 호출하며 문제를 해결하는 방식이랍니다. 재귀 함수를 만들 때는 다음 두 가지가 꼭 필요해요!

*   **바닥 조건 (Base Case)**: 재귀 호출을 멈추는 조건이에요. 이 조건이 없으면 함수가 끝없이 호출되다가 에러(무한 루프)가 발생한답니다.
*   **재귀 호출 (Recursive Call)**: 자기 자신을 다시 호출하는 부분으로, 문제가 점진적으로 작아지면서 바닥 조건에 도달하도록 해야 해요.

---

## 재귀 함수 예시

### 1. 1부터 N까지의 합 구하기

`sum(n) = 1 + 2 + ... + (n-1) + n`
이 식은 `sum(n) = sum(n-1) + n` 이라는 재귀적인 관계로 표현할 수 있어요!

```python
def sum_n(n):
    if n == 1: # 바닥 조건 (Base Case): n이 1이면 1을 반환하고 재귀를 멈춰요.
        return 1
    # 재귀 호출: sum(n-1)을 호출하고 현재 n을 더해요.
    return sum_n(n-1) + n

# 수행 시간 (Time Complexity): O(n)
# T(n) = T(n-1) + C (n번 재귀 호출)
```

---

### 2. 특정 범위 내 숫자의 합 구하기 (`a`부터 `b`까지)

`sum_range(a, b) = a + (a+1) + ... + (b-1) + b` (가정: `a <= b`)
이 문제는 '분할 정복(Divide and Conquer)' 방식으로 해결할 수 있어요.
`sum_range(3, 8) = 3 + 4 + ... + 7 + 8`
`sum_range(a, b) = sum_range(a, m) + sum_range(m+1, b)` (단, `m = (a+b) // 2`)

```python
def sum_range(a, b):
    if a == b: # 바닥 조건 1: 시작과 끝이 같으면 해당 숫자만 반환
        return a
    if a > b:  # 바닥 조건 2: 시작이 끝보다 크면 합이 0 (유효하지 않은 범위)
        return 0
    
    # 중간 지점 m을 기준으로 범위를 둘로 나눠 재귀 호출해요.
    m = (a + b) // 2 
    return sum_range(a, m) + sum_range(m + 1, b)

# 수행 시간 (Time Complexity): O(n)
# T(n) = T(n/2) + T(n/2) + C = 2*T(n/2) + C (분할된 n에 대해 호출)
```

---

### 3. 리스트 뒤집기 (In-place Reversal)

`A = [1, 2, 3, 4, 5]` 를 `A = [5, 4, 3, 2, 1]`로 뒤집는 예시예요.
재귀적으로 뒤집는 함수는 보통 시작과 끝 인덱스를 받아 서로의 위치를 바꿔주는 방식으로 구현한답니다.

```python
def reverse_list_inplace(arr, start, end):
    # 바닥 조건: 시작 인덱스가 끝 인덱스보다 크거나 같으면 (더 이상 바꿀 요소가 없으면) 재귀를 멈춰요.
    if start >= end:
        return

    # 첫 번째 요소와 마지막 요소의 위치를 바꿔요 (Swap).
    arr[start], arr[end] = arr[end], arr[start]

    # start를 하나 늘리고, end를 하나 줄여서 다음 안쪽 요소들을 재귀적으로 바꿔요.
    reverse_list_inplace(arr, start + 1, end - 1)

# 사용 예시:
my_list = [1, 2, 3, 4, 5]
print(f"원본 리스트: {my_list}")
reverse_list_inplace(my_list, 0, len(my_list) - 1)
print(f"뒤집은 리스트: {my_list}") # 결과: [5, 4, 3, 2, 1]

# 수행 시간 (Time Complexity): O(n)
# (n/2)번의 swap 연산이 발생하고 각 연산은 상수 시간)
```
*(참고: 파이썬에서 `reverse(A[1:]) + A[:1]` 방식은 새로운 리스트를 계속 만들어서 메모리 효율이 좋지 않을 수 있어, 보통 In-place (제자리) 방식으로 `start`와 `end` 인덱스를 이용해 바꾼답니다.)*