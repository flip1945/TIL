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
~~~

더 간결한 방식으로 작성한 코드

~~~python
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
