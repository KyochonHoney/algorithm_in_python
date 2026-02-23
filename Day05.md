# Day05.

# 1. 큰 두 수의 곱셈: 분할정복 알고리즘 (Karatsuba's algorithm)
# 수행시간 T(n): 점화식 (recurrence equation)으로 표현
#
# Karatsuba 알고리즘은 두 n자리 수를 곱할 때 두 부분으로 나누어 계산하는 방식으로
# 전통적인 O(n^2) 곱셈보다 빠른 O(n^{log_2 3}) ≈ O(n^{1.585}) 시간복잡도를 가집니다.

# 2. 이진 탐색 (binary search)
# 정렬된 배열에서 요소 k를 찾는 알고리즘 (시간복잡도 O(log n))

def binarySearch(A, a, b, k):
    if a > b:
        return -1
    m = (a + b) // 2
    if A[m] == k:
        return m
    elif A[m] > k:
        return binarySearch(A, a, m - 1, k)
    else:
        return binarySearch(A, m + 1, b, k)

# 시간 복잡도 점화식
# 이진 탐색: T(n) = T(n/2) + C
# 퀵 셀렉트(best case): T(n) = 2T(n/2) + n