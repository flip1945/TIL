# 타겟넘버(Level 3)

### 문제 설명

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.   

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

---

#### 제한사항

* 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
* 주어진 공항 수는 3개 이상 10,000개 이하입니다.
* tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
* 주어진 항공권은 모두 사용해야 합니다.
* 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
* 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

---

#### 입출력 예
|tickets                                                                        |	return                                    |
|-------------------------------------------------------------------------------|-------------------------------------------|
|[["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]                               |	["ICN", "JFK", "HND", "IAD"]              |
|[["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL","SFO"]]|	["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"]|

---

#### 입출력 예 설명

예제 #1

["ICN", "JFK", "HND", "IAD"] 순으로 방문할 수 있습니다.

예제 #2

["ICN", "SFO", "ATL", "ICN", "ATL", "SFO"] 순으로 방문할 수도 있지만 ["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"] 가 알파벳 순으로 앞섭니다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/43164)

---

### 문제풀이

완전탐색을 해야 할 것으로 판단됐습니다.   

주어진 공항 수는 3개 이상 10,000개 이하로 되어있는데, tickets의 개수는 얼마인지 주어져 있지 않았습니다.   

하지만 모든 티켓을 소모해야 하는 문제라서 시간 복잡도는 O(N)이면 풀 수 있을 것으로 보였습니다.   

모든 도시를 방문할 수 없는 경우는 없기 때문에 경로가 있는 지 없는 지 판별하지 않아도 될 것이라고 생각했는데, 착오였습니다.   

문제에서 주어진 제한사항을 모두 따랐다고 생각했는데, 예외 케이스가 존재했습니다.   

[['ICN','BOO' ], [ 'ICN', 'COO' ], [ 'COO', 'DOO' ], ['DOO', 'COO'], [ 'BOO', 'DOO'] ,['DOO', 'BOO'], ['BOO', 'ICN' ], ['COO', 'BOO']]   

이렇게 티켓이 주어지면   

ICN -> BOO -> DOO -> BOO -> ICN -> COO -> BOO -> X   

더 이상 진행불가가 되면서 정답과 다른 결과가 나왔습니다.   

즉, 다른 경로가 존재하는지 DFS와 백트래킹으로 확인해야 했던 것입니다.   

하지만 저는 그런 방식으로 구현하지 않아서 약간 억지로 문제를 해결했습니다.

다른 분들이 DFS와 백트래킹, 혹은 스택으로 쉽게 풀이한 것을 보니 감탄이 나왔습니다.

---

#### 나의 풀이
~~~python
from collections import deque


def solution(tickets):
    answer = ['ICN']
    # 공항 정보 생성
    airports = get_airports(tickets)
    # 각 공항에서 갈 수 있는 공항 딕셔너리 생성
    airport_table = create_airport_table(create_tickets_dict(airports), sorted(tickets))
    # 첫 공항은 ICN 으로 설정
    cur_airport = "ICN"
    # 예외 처리를 위한 변수
    _next_airport = ""

    while len(answer) < len(tickets):
        next_airport = airport_table[cur_airport].popleft()

        # 다음 공항이 계속 같아서 무한 루프에 빠지게 되면 예외처리
        if _next_airport == next_airport:
            answer.pop()
            airport_table[answer[-1]].append(cur_airport)
            airport_table[cur_airport].append(next_airport)
            cur_airport = answer[-1]
        # 다음 공항이 마지막 공항인지를 판단
        if not airport_table[next_airport]:
            airport_table[cur_airport].append(next_airport)
            _next_airport = next_airport
        # 아닌 경우 결과에 다음에 갈 공항을 입력 하고, 현재 공항을 갱신
        else:
            answer.append(next_airport)
            cur_airport = next_airport

    answer.append(airport_table[cur_airport][0])
    return answer


def get_airports(tickets):
    return set([ticket[0] for ticket in tickets]) | set([ticket[1] for ticket in tickets])


def create_tickets_dict(airports):
    airport_dict = {}
    for airport in sorted(list(airports)):
        airport_dict[airport] = deque()
    return airport_dict


def create_airport_table(air_dict, tickets):
    for ticket in tickets:
        air_dict[ticket[0]].append(ticket[1])
    return air_dict


if __name__ == "__main__":
    solution([["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]])
~~~

---

#### 다른 사람의 풀이

1. 스택을 이용한 풀이

   출처 : https://copy-driven-dev.tistory.com/entry/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-ProgrammersPython-%EC%97%AC%ED%96%89%EA%B2%BD%EB%A1%9C

~~~python
def solution(tickets):
    t = dict()
    for ticket in tickets:
        if ticket[0] in t:
            t[ticket[0]].append(ticket[1])
        else:
            t[ticket[0]] = [ticket[1]]
            
    for k in t.keys():
        t[k].sort(reverse=True)
        
    st = ["ICN"]
    answer = []
    
    while st:
        top = st[-1]
        
        if top not in t or len(t[top]) == 0:
            answer.append(st.pop())
        else:
            st.append(t[top][-1])
            t[top].pop()
            
    answer.reverse()
    
    return answer
~~~

2. 재귀를 이용한 풀이 

   출처 : https://lsh424.tistory.com/58

~~~python
import collections, copy

def solution(tickets):
	
    # 그래프 만들기
    graph = collections.defaultdict(list)

    for ticket in tickets:
        graph[ticket[0]].append(ticket[1])
	
    # 출발점이 무조건 'ICN'
    queue = collections.deque([('ICN', ['ICN'])])
    result = []
	
    def find_path(graph, queue):
        n, path = queue.popleft()
		
        # 재귀 종료조건
        if len(path) == len(tickets)+1:
            result.append(path) # 완성된 경로 추가
            return

        for m in graph[n]:
            next_graph = copy.deepcopy(graph) # 그래프 깊은 복사
            queue.append((m, path + [m])) # 경로 분기
            next_graph[n].remove(m) # 방문한 공항 삭제
            find_path(next_graph,queue) # 재귀

    find_path(graph,queue)       
    result.sort() # 알파벳순 정렬
    return result[0]
~~~

3. DFS, 백트래킹을 이용한 풀이

   출처 : https://inspirit941.tistory.com/164

~~~python
from collections import defaultdict
import copy
def dfs(start, table, arr, result, n_tickets):
    # 이동한 경로를 저장함
    arr.append(start)
    for i in range(len(table[start])):
        # 이미 방문했으면 건너뛴다
        if table[start][i] == True:
            continue
        else:
            # 해당 경로에서 도착할 수 있는 다음 경로를 확인한다
            next_des = table[start][i]
            # 다음 경로의 visited여부를 True로 반환한다.
            table[start][i] = True
            # 다음 경로에서 진행할 수 있는 여행경로를 탐색한다.
            temp = dfs(next_des, table, arr, result, n_tickets)
            # 주어진 항공권을 모두 사용해 경로를 만들 수 있는 경우
            if len(temp) == n_tickets+1:
                result.append(copy.deepcopy(temp))
            # 백트래킹
            arr.pop()
            table[start][i] = next_des
    
    # 해당 공항에서 더 이상 다음으로 진행할 수 없으면 저장한 경로를 반환
    return arr
    
def solution(tickets):
    # 각 공항에서 다른 곳으로 이동할 수 있는 경우의 수 저장
    table = defaultdict(list)
    
    for value in tickets:
        f, t = value[0], value[1]
        table[f].append(t)
    
    # 방문할 수 있는 경로를 저장하는 배열    
    result = []
    dfs('ICN', table, [], result, len(tickets))
    
    # 알파벳 순서가 앞서는 경로 출력하기
    return sorted(result)[0]
~~~

