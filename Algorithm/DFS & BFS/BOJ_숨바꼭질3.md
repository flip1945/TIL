# 숨바꼭질3 (Gold 5)

### 문제

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 0초 후에 2\*X의 위치로 이동하게 된다.   

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.   

---

#### 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.   

---

#### 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.     

---

#### 예제 입력 1
~~~
5 17  
~~~

#### 예제 출력 1
~~~
2
~~~

출처 : https://www.acmicpc.net/problem/13549

---

### 문제풀이

이 문제는 bfs 문제입니다.   

이전에 풀었던 숨바꼭질 문제의 확장판인데, 이번에는 순간 이동할 때 걸리는 시간이 0초입니다.   

저는 처음에 풀 때는 순간 이동 부분과 걷는 부분을 따로 처리했는데, 그렇게 해서 더 복잡해지고 정답 맞추기도 어려웠습니다.   

그래서 다 푼 후에 순간 이동 부분과 걷는 부분 처리를 같이 하는 방식으로 다시 풀었습니다.   

---

#### 나의 풀이

1. 정답으로 제출한 풀이

~~~python
from collections import deque

n, k = map(int, input().split())
# 시작점이 도착점보다 더 큰 경우 예외 처리
if n >= k:
    print(n - k)
    exit(0)
# 시간을 저장할 리스트를 생성
dp = [-1] * 100001
# bfs를 위한 queue를 생성
que = deque([(n, 0)])
# bfs 실행
while que:
    # queue에서 pop
    cur, sec = que.popleft()
    # 현재 위치가 도착점의 위치라면 종료
    if cur == k:
        print(sec)
        break
    # 현재 위치의 시간을 갱신
    dp[cur] = sec
    # 순간 이동시 배로 늘어나기 때문에 2^n 형태로 반복함
    for i in range(1, 17):
        j = cur * (2**i)
        # 좌표를 벗어나는 경우, 이미 방문했던 적이 있던 경우는 생략
        if j > 100000 or dp[j] != -1:
            continue
        que.append((j, sec))
    # 걷는 부분 실행
    for next in [cur - 1, cur + 1]:
        # 좌표를 벗어나거나 이미 방문했던 적이 있던 경우는 생략
        if next > 100000 or next < 0 or dp[next] != -1:
            continue
        
        que.append((next, sec + 1))
~~~

2. 제출 후 다듬은 풀이

~~~python
from collections import deque

n, k = map(int, input().split())

dp = [-1] * 100001
dp[n] = 0
que = deque([n])

while que:
    cur = que.popleft()
    
    if cur == k:
        print(dp[cur])
        break
    # 순간 이동, 걷기 부분을 한 번에 처리
    for sec, next in [(0, cur * 2), (1, cur - 1), (1, cur + 1)]:
        if 0 <= next <= 100000 and dp[next] == -1:
            dp[next] = dp[cur] + sec
            que.append(next)
~~~
