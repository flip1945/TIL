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

ph = [[]] # 생략

58

    visited = [False] * 9

59

    bfs(graph, 1, visited)

60

~~~

61



62

<hr/>

63



64

### Sort

65



66

#### 선택 정렬(Selection Sort) 알고리즘

67



68

~~~python

69

array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

70



71

for i in range(len(array) - 1):

72

    min_index = i

73

    for j in range(i + 1, len(array)):

74

        if array[min_index] > array[j]:

75

            min_index = j

76

    # swap

77

    array[i], array[min_index] = array[min_index], array[i]

78

    

79

print(array)

80

~~~

81



82

#### 삽입 정렬(Insertion Sort) 알고리즘

83



84

~~~python

85

array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

86



87

for i in range(i, len(array)):

88

    for j in range(i, 0, -1):

89

        # 현재 삽입해야 하는 원소가 삽입된 원소보다 작다면 

90

        if array[j] < array[j-1]:

91

            # 왼쪽으로 한 칸 이동

92

            array[j], array[j-1] = array[j-1], array[j]

93

        else:

94

            braek

95

    

96

print(array)

97

~~~

98



99

#### 퀵 정렬(Quick Sort) 알고리즘


