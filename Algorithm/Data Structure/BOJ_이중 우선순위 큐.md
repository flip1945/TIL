# 이중 우선순위 큐 (Gold 5)

### 문제

이중 우선순위 큐(dual priority queue)는 전형적인 우선순위 큐처럼 데이터를 삽입, 삭제할 수 있는 자료 구조이다. 전형적인 큐와의 차이점은 데이터를 삭제할 때 연산(operation) 명령에 따라 우선순위가 가장 높은 데이터 또는 가장 낮은 데이터 중 하나를 삭제하는 점이다. 이중 우선순위 큐를 위해선 두 가지 연산이 사용되는데, 하나는 데이터를 삽입하는 연산이고 다른 하나는 데이터를 삭제하는 연산이다. 데이터를 삭제하는 연산은 또 두 가지로 구분되는데 하나는 우선순위가 가장 높은 것을 삭제하기 위한 것이고 다른 하나는 우선순위가 가장 낮은 것을 삭제하기 위한 것이다.   

정수만 저장하는 이중 우선순위 큐 Q가 있다고 가정하자. Q에 저장된 각 정수의 값 자체를 우선순위라고 간주하자.   

Q에 적용될 일련의 연산이 주어질 때 이를 처리한 후 최종적으로 Q에 저장된 데이터 중 최댓값과 최솟값을 출력하는 프로그램을 작성하라.

---

#### 입력

입력 데이터는 표준입력을 사용한다. 입력은 T개의 테스트 데이터로 구성된다. 입력의 첫 번째 줄에는 입력 데이터의 수를 나타내는 정수 T가 주어진다. 각 테스트 데이터의 첫째 줄에는 Q에 적용할 연산의 개수를 나타내는 정수 k (k ≤ 1,000,000)가 주어진다. 이어지는 k 줄 각각엔 연산을 나타내는 문자(‘D’ 또는 ‘I’)와 정수 n이 주어진다. ‘I n’은 정수 n을 Q에 삽입하는 연산을 의미한다. 동일한 정수가 삽입될 수 있음을 참고하기 바란다. ‘D 1’는 Q에서 최댓값을 삭제하는 연산을 의미하며, ‘D -1’는 Q 에서 최솟값을 삭제하는 연산을 의미한다. 최댓값(최솟값)을 삭제하는 연산에서 최댓값(최솟값)이 둘 이상인 경우, 하나만 삭제됨을 유념하기 바란다.   

만약 Q가 비어있는데 적용할 연산이 ‘D’라면 이 연산은 무시해도 좋다. Q에 저장될 모든 정수는 32-비트 정수이다. 

---

#### 출력

출력은 표준출력을 사용한다. 각 테스트 데이터에 대해, 모든 연산을 처리한 후 Q에 남아 있는 값 중 최댓값과 최솟값을 출력하라. 두 값은 한 줄에 출력하되 하나의 공백으로 구분하라. 만약 Q가 비어있다면 ‘EMPTY’를 출력하라.

---

#### 예제 입력 1
~~~
2
7
I 16
I -5643
D -1
D 1
D 1
I 123
D -1
9
I -45
I 653
D 1
I -642
I 45
I 97
D 1
D -1
I 333
~~~

#### 예제 출력 1
~~~
EMPTY
333 -45
~~~

[출처](https://www.acmicpc.net/problem/7662)

---

### 문제풀이

이번 문제는 우선순위 큐(힙)를 이용해 푸는 문제입니다.   

최소힙과 최대힙을 동시에 구현해야 하는데,   

힙을 따로 만들게 되면 이미 지워진 요소를 다른 힙에서 인식하지 못하는 경우가 생기기 때문에 주의해서 구현해줘야 합니다.   

제가 이 문제를 해결한 방식은 힙에 요소를 넣을 때, 튜플 형태로 식별할 수 있는 인덱스를 뒤에 붙여서 같이 삽입해줬습니다.   

이렇게 하면 다른 힙에서 지워진 요소를 무시하고 구현할 수 있기 때문에 문제를 해결할 수 있습니다.

---

#### 나의 풀이

~~~python
import heapq
import sys

for T in range(int(input())):
    k = int(input())
    # 방문 여부를 확인하는 리스트
    visited = [False] * k

    max_heap = []
    min_heap = []

    for i in range(k):
        cmd, num = sys.stdin.readline().split()
        num = int(num)

        # 입력 명령이 들어오면 요소를 최소힙, 최대힙에 동시에 삽입
        if cmd == "I":
            heapq.heappush(min_heap, (num, i))
            heapq.heappush(max_heap, (-num, i))
        # 최대힙 출력 명령이 들어오면 최대힙에서 요소를 꺼냄
        elif cmd == "D" and num == 1:
            # 최소힙에서 이미 꺼낸 요소는 무시함
            while max_heap and visited[max_heap[0][1]]:
                heapq.heappop(max_heap)
            # 무시한 후 꺼낼 요소가 있다면 꺼내고, 해당 인덱스를 방문 처리함
            if max_heap:
                visited[max_heap[0][1]] = True
                heapq.heappop(max_heap)
        # 최소힙 출력 명령이 들어오면 최소힙에서 요소를 꺼냄
        else:
            # 최대힙에서 이미 꺼낸 요소는 무시함
            while min_heap and visited[min_heap[0][1]]:
                heapq.heappop(min_heap)
            # 무시한 후 꺼낼 요소가 있다면 꺼내고, 해당 인덱스를 방문 처리함
            if min_heap:
                visited[min_heap[0][1]] = True
                heapq.heappop(min_heap)
    a, b = 0, 0

    while max_heap and visited[max_heap[0][1]]:
        heapq.heappop(max_heap)
    if max_heap:
        a = -max_heap[0][0]
    else:
        print("EMPTY")
        continue

    while min_heap and visited[min_heap[0][1]]:
        heapq.heappop(min_heap)
    if min_heap:
        b = min_heap[0][0]
    print(a, b)
~~~

---

#### 다른 사람의 풀이

~~~python
from heapq import heappush, heappop
import sys

input = sys.stdin.readline

def pop(hq):
    global check
    while hq:
        n, idx = heappop(hq)
        if not check[idx]:
            check[idx] = True
            return n

for _ in range(int(input())):
    N = int(input())
    max_heap = []
    min_heap = []
    check = [False] * N
    cnt = 0
    for i in range(N):
        t, num = input().split()
        num = int(num)
        if t == 'I':
            cnt += 1
            heappush(max_heap, (-num, i))
            heappush(min_heap, (num, i))
        else:
            if num > 0:
                pop(max_heap)
            else:
                pop(min_heap)
    if sum(check) == cnt:
        print('EMPTY')
    elif sum(check) == cnt - 1:
        a = pop(min_heap)
        print(a, a)
    else:
        print(-pop(max_heap), pop(min_heap))
~~~
