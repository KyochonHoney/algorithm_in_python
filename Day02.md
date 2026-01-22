#   Day02.
## 선택 ( Selection ):
*   **입력**: n개의 값과 K (1 <= k <= n) 값
*   **출력**: k번째로 작은 입력 값
*   **목표**: 비교 횟수 최소화

*   k = n일 때, 최댓값 찾기 : n-1 번 비교 필요, 충분
*   k = 1, n일 때, 최댓값, 최솟값 동시 찾기
*   k = 1, 2일 때, 제일 작은 값, 두 번째로 작은 값 
    *   모두 O(n)에서 끝남

### - Quick Select
1.  `p`를 고른다: `pivot`
2.  원본 배열을 `pivot` 기준으로 세 그룹으로 나눈다:
    *   `A` = {`p`보다 작은 값}
    *   `B` = {`p`보다 큰 값}
    *   `M` = {`p`와 같은 값}  
3.  `k`의 위치에 따라 재귀적으로 탐색한다:
    *   `if len(A) > k:` : `k`번째 값은 `A` 안에 있다. (`M`과 `B`에는 없음) -> `A`에서 재귀적으로 찾기
    *   `elif len(A) + len(M) < k:` : `k`번째 값은 `B` 안에 있다. (`A`와 `M`의 크기만큼 `k` 조정 필요) -> `B`에서 재귀적으로 찾기
    *   `else:` (`len(A) <= k <= len(A) + len(M)`) : `k`번째 값은 `M` 안에 있다. -> `return p` (피벗이 정답)

```python
def quick_select(L, k): # L = list, k = 자연수 (0-indexed 또는 1-indexed에 따라 달라짐)
    # 편의상 k를 0-indexed로 가정하고 구현합니다. (k번째 작은 값은 arr[k-1]에 해당)
    if not L:
        return None
    
    p = L[0] # 첫 번째 원소를 피벗으로 선택
    A, B, M = [], [], []
    for x in L:
        if x < p: # p보다 작은 값
            A.append(x)
        elif x > p: # p보다 큰 값
            B.append(x)
        else: # p와 같은 값
            M.append(x)
            
    if k < len(A): # k번째 원소가 A 그룹에 있는 경우 (k는 0부터 시작)
        return quick_select(A, k)
    elif k < len(A) + len(M): # k번째 원소가 M 그룹에 있는 경우
        return p # 피벗이 정답
    else: # k번째 원소가 B 그룹에 있는 경우
        # B 그룹에서 찾을 때는 k를 B 그룹의 상대적인 인덱스로 조정해야 합니다.
        return quick_select(B, k - (len(A) + len(M))) 
```

*   **점화식**: 
    *   Worst Case: `T(n) = T(n-1) + O(n)` -> `O(n^2)`
    *   Best Case: `T(n) = T(n/2) + O(n)` -> `O(n)`
    *   Average Case: `O(n)`

### - Median of Medians(MoM)
MoM(`L`, `k`):
1.  입력 리스트 `L`을 5개씩 그룹으로 나눈다.
2.  각 그룹을 정렬하고, 각 그룹의 중앙값을 찾는다. (이들을 모아 새로운 리스트 `medians` 생성)
3.  `m` = `MoM(medians, |medians| / 2)` : `medians` 리스트에 대해 재귀적으로 `MoM`을 호출하여 '중앙값들의 중앙값'을 구한다. 이 `m`이 이번 단계의 **최적 피벗**이 된다.
4.  구해진 `m`을 `pivot`으로 사용하여 Quick Select의 방식으로 `L`을 세 부분 (`less`, `equal`, `greater`)으로 나눈다.
5.  `k`의 위치에 따라 (`less`, `equal`, `greater` 중) 적절한 부분 리스트에 대해 재귀 호출하거나 `m`을 반환한다.
        
*   **점화식**: `T(n) = T(n/5) + T(7n/10) + O(n)` -> `O(n)`
    (이론적으로 최악의 경우에도 항상 선형 시간 복잡도 `O(n)`을 보장한다.)