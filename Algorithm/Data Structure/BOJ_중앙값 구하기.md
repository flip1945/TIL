# 중앙값 구하기

### 문제

어떤 수열을 읽고, 홀수번째 수를 읽을 때 마다, 지금까지 입력받은 값의 중앙값을 출력하는 프로그램을 작성하시오.   

예를 들어, 수열이 1, 5, 4, 3, 2 이면, 홀수번째 수는 1번째 수, 3번째 수, 5번째 수이고, 1번째 수를 읽었을 때 중앙값은 1, 3번째 수를 읽었을 때는 4, 5번째 수를 읽었을 때는 3이다.   

---

#### 입력

첫째 줄에 테스트 케이스의 개수 T(1 ≤ T ≤ 1,000)가 주어진다. 각 테스트 케이스의 첫째 줄에는 수열의 크기 M(1 ≤ M ≤ 9999, M은 홀수)이 주어지고, 그 다음 줄부터 이 수열의 원소가 차례대로 주어진다. 원소는 한 줄에 10개씩 나누어져있고, 32비트 부호있는 정수이다.

---

#### 출력

각 테스트 케이스에 대해 첫째 줄에 출력하는 중앙값의 개수를 출력하고, 둘째 줄에는 홀수 번째 수를 읽을 때 마다 구한 중앙값을 차례대로 공백으로 구분하여 출력한다. 이때, 한 줄에 10개씩 출력해야 한다.

[출처](https://www.acmicpc.net/problem/2696)

---

### 문제풀이

연속으로 입력되는 값에서 연속으로 중앙값을 찾아서 출력해야 하는 문제입니다.   

이 문제를 풀려면 최대힙, 최소힙 두 개를 사용해야 합니다.   

중앙값을 기준으로 왼쪽은 최대 힙, 오른쪽은 최소 힙으로 설정합니다.  

중앙값보다 작은 값은 왼쪽힙(최대힙)에 넣고, 큰 값은 오른쪽(최소힙)에 넣습니다.   

홀수 번째 숫자마다 들어오는 값은 총 2개이기 때문에, 매번 출력할 때마다 3가지 선택지를 고를 수 있습니다.   

1. 왼쪽에 두 개가 들어오는 경우

2. 왼쪽, 오른쪽 한 개씩 들어오는 경우

3. 오른쪽에 두 개가 들어오는 경우

1번의 경우는 왼쪽힙(최대힙)의 값을 중앙값으로 변경하고, 원래의 중앙값을 오른쪽힙(최소힙)에 넣습니다.   

2번의 경우는 바뀐 것이 없기 때문에 그대로 두면 됩니다.   

3번의 경우는 오른쪽힙(최소힙)의 값을 중앙값으로 변경하고, 원래의 중앙값을 왼쪽힙(최대힙)에 넣습니다.   

~~~python
import heapq

test_case = int(input())

for _ in range(test_case):
    n = int(input())
    # 수열을 num 리스트에 저장
    num = []
    for i in range(n // 10):
        num += list(map(int, input().split()))
    if n % 10:
        num += list(map(int, input().split()))
    # 첫 값을 중앙값에 넣고 출력
    mid = num[0]
    # mid보다 큰 값이 저장될 heap
    min_heap = []
    # mid보다 작은 값이 저장될 heap
    max_heap = []
    # 결과를 저장할 리스트
    result = [mid]

    for i in range(1, n):
        # 현재 num의 값이 mid 보다 큰지 작은지 확인해서 max, min heap에 저장
        heapq.heappush(min_heap, num[i]) if num[i] > mid else heapq.heappush(max_heap, -num[i])
        # 홀수 번째 수인 경우
        if not i % 2:
            # mid보다 큰 값이 더 많다면
            if len(min_heap) > len(max_heap):
                tmp = mid
                mid = heapq.heappop(min_heap)
                heapq.heappush(max_heap, -tmp)
            # mid보다 작은 값이 더 많다면
            elif len(max_heap) > len(min_heap):
                tmp = mid
                mid = -heapq.heappop(max_heap)
                heapq.heappush(min_heap, tmp)
            result.append(mid)
    # 결과 출력
    print(len(result), end='')
    for i in range(len(result)):
        if i % 10 == 0:
            print()
        print(result[i], end=' ')
~~~
