# Algorithm   

알고리즘 공부입니다.  

TDD 방식을 적용해 알고리즘 공부합니다.

## 공부할 목록

* Greedy
* DFS & BFS
* Sort
* Binary Search
* Dinamic Programming
* Graph

## 예제 소스 코드

### DFS & BFS

#### DFS 알고리즘

~~~python
def dfs(graph, v, visited):
    # 현재 노드 방문 처리
    visited[v] = True
    print(v, end=' ')
    # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
        if not visited[i]:
           dfs(graph, i, visited)
           
    graph = [[]] # 생략
    visited = [False] * 9
    dfs(graph, 1, visited)
~~~

#### BFS 알고리즘

~~~python
from collections import deque

def bfs(graph, start, visited):
    queue = deque([start])
    # 현재 노드 방문 처리
    visited[start] = True
    # 큐가 빌 때까지 반복
    while queue:
        # 큐에서 원소 하나를 빼서 출력
        v = queue.popleft()
        print(v, end=' ')
        # 아직 방문하지 않은 인접한 원소를 모두 큐에 삽입
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True
           
    graph = [[]] # 생략
    visited = [False] * 9
    bfs(graph, 1, visited)
~~~

<hr/>

### Sort

#### 선택 정렬(Selection Sort) 알고리즘

~~~python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array) - 1):
    min_index = i
    for j in range(i + 1, len(array)):
        if array[min_index] > array[j]:
            min_index = j
    # swap
    array[i], array[min_index] = array[min_index], array[i]
    
print(array)
~~~

#### 삽입 정렬(Insertion Sort) 알고리즘

~~~python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(i, len(array)):
    for j in range(i, 0, -1):
        # 현재 삽입해야 하는 원소가 삽입된 원소보다 작다면 
        if array[j] < array[j-1]:
            # 왼쪽으로 한 칸 이동
            array[j], array[j-1] = array[j-1], array[j]
        else:
            braek
    
print(array)
~~~

#### 퀵 정렬(Quick Sort) 알고리즘

~~~python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array, start, end):
    # 원소가 1개인 경우 종료
    if start >= end:
        return
    pivot = start
    left = start + 1
    right = end
    while left <= right:
        # 피벗보다 큰 데이터를 찾을 때까지 반복
        while left <= end and array[left] < array[pivot]:
            left += 1
        # 피벗보다 작은 데이터를 찾을 때까지 반복
        while right > start and array[right] >= array[pivot]:
            right -= 1
        # 엇갈렸다면 피벗을 교체
        if left > right:
            array[right], array[pivot] = array[pivot], array[right]
        # 엇갈리지 않았다면 작은 데이터와 큰 데이터를 교체
        else:
            array[left], array[right] = array[right], array[left]
    # 피벗을 기준으로 왼쪽과 오른쪽을 분할해서 각각 정렬 실행
    quick_sort(array, start, right - 1)
    quick_sort(array, right + 1, end)
    
quick_sort(array, 0, len(array) - 1)
print(array)

# 더 간결한 방식으로 작성한 코드

array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array):
    # 원소가 1개 이하인 경우 종료
    if len(array) <= 1:
        return array
    pivot = array[0]
    tail = array[1:]
    
    left_side = [x for x in tail if x <= pivot]
    right_side = [x for x in tail if x > pivot]
    
    # 피벗을 기준으로 왼쪽과 오른쪽을 분할해서 각각 정렬 실행
    return quick_sort(left_side) + [pivot] + quick_sort(right_side)
    
print(quick_sort(array))
~~~

#### 계수 정렬(Counting Sort) 알고리즘

~~~python
array = [7, 5, 9, 0, 3, 1, 6, 2, 9, 1, 4, 8, 0, 5]

def counting_sort(array):
    result = []
    count = [0] * (max(array) + 1)

    for i in range(len(array)):
        # 각 데이터에 해당하는 인덱스의 값 증가
        count[array[i]] += 1

    for i in range(len(count)):
        for j in range(count[i]):
            result.append(i)
    return result
    
print(counting_sort(array))
~~~

<hr/>

### Binary Search

#### Binary Search 알고리즘

~~~python
array = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    if array[mid] == target:
        return mid
    # 찾고자 하는 값이 더 작을 경우
    elif array[mid] > target:
        # 중간보다 왼쪽을 탐색
        return binary_search(array, target, start, mid - 1)
    # 찾고자 하는 값이 더 큰 경우
    else:
        # 중간보다 오른쪽을 탐색
        return binary_search(array, target, mid + 1, end)

print(binary_search(array, 7, 0, len(array) - 1))

# 반복문으로 구현

array = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        if array[mid] == target:
            return mid
        # 찾고자 하는 값이 더 작을 경우 end를 중간값 보다 1작게 설정
        elif array[mid] > target:
            end = mid - 1
        # 찾고자 하는 값이 더 클 경우 start를 중간값보다 1크게 설정
        else:
            start = mid + 1
    return None

print(binary_search(array, 7, 0, len(array) - 1))
~~~

#### Bisect 라이브러리 사용법

~~~python
from bisect import bisect_left, bisect_right

array = [1, 2, 3, 3, 3, 3, 4, 4, 8, 9]

def count_by_range(array, left_value, right_value):
    right_index = bisect_right(array, right_value)
    left_index = bisect_left(array, left_value)
    return right_index - left_index

# 값이 4인 데이터의 개수 출력
print(count_by_range(array, 4, 4))
# 값이 -1 이상, 3 이하의 데이터 개수 출력
print(count_by_range(array, -1, 3))
~~~

<hr/>

### Graph

#### 다익스트라(Dijkstra) 알고리즘

~~~python
import heapq

INF = int(1e9)

# n : 노드의 개수, m : 간선의 개수, start : 시작 노드
n, m = map(int, input().split())
start = int(input)

graph = [[] for i in range(n + 1)]

distance = [INF] * (n + 1)

for _ in range(m):
    a, b, c = map(int, input().split())
    # a번 노드에서 b번 노드로 가는 비용이 c라는 의미
    graph[a].append((b, c))

def dijkstra(start):
    q = []
    # 시작 노드까지의 최단 거리는 0이라는 의미로 최소 힙(우선 순위 큐)에 삽입
    heapq.heappush(q, (0, start))
    distance[start] = 0
    while q:
        # dist : now 노드 까지의 최단 거리, now : 현재 노드 번호
        dist, now = heapq.heappop(q)
        # 현재 노드에 저장된 최단 거리가 우선 순위 큐에서 나온 최단 거리보다 작다면, 이미 처리된 노드로 간주함
        if distance[now] < dist:
            continue
        # 현재 노드와 인접한 노드들을 확인
        for i in graph[now]:
            cost = dist + i[1]
            # 현재 노드를 거쳐서, 다른 인접 노드로 이동하는 거리가 더 짧다면, 최단 거리를 갱신하고 우선 순위 큐에 입력
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))
                
dijkstra(start)

for i in range(1, n + 1):
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])
~~~

#### 플로이드 워셜(Floyd-Warshall) 알고리즘

~~~python
INF = int(1e9)

# n : 노드의 개수, m : 간선의 개수
n, m = map(int, input().split())
# 2차원 리스트를 만들고 무한으로 초기화
graph = [[INF] * (n + 1) for _ in range(n + 1)]

# 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

# 각 간선에 대한 정보를 입력
for _ in range(m):
    # a번 노드에서 b번 노드로 가는 비용이 c라는 의미
    a, b, c = map(int, input().split())
    graph[a][b] = c
    
for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])
            
for a in range(1, n + 1):
    for b in range(1, n + 1):
        if graph[a][b] == INF:
            print("INFINITY", end=' ')
        else:
            print(graph[a][b], end=' ')
    print()
~~~
