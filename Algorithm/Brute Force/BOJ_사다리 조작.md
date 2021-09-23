# 사다리 조작 (Gold 4)

### 문제 설명

사다리 게임은 N개의 세로선과 M개의 가로선으로 이루어져 있다. 인접한 세로선 사이에는 가로선을 놓을 수 있는데, 각각의 세로선마다 가로선을 놓을 수 있는 위치의 개수는 H이고, 모든 세로선이 같은 위치를 갖는다. 아래 그림은 N = 5, H = 6 인 경우의 그림이고, 가로선은 없다.   

<p align="center">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/1.png" width="400">
</p>

초록선은 세로선을 나타내고, 초록선과 점선이 교차하는 점은 가로선을 놓을 수 있는 점이다. 가로선은 인접한 두 세로선을 연결해야 한다. 단, 두 가로선이 연속하거나 서로 접하면 안 된다. 또, 가로선은 점선 위에 있어야 한다.   

<p align="center">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/2.png" width="400">
</p>

위의 그림에는 가로선이 총 5개 있다. 가로선은 위의 그림과 같이 인접한 두 세로선을 연결해야 하고, 가로선을 놓을 수 있는 위치를 연결해야 한다.   

사다리 게임은 각각의 세로선마다 게임을 진행하고, 세로선의 가장 위에서부터 아래 방향으로 내려가야 한다. 이때, 가로선을 만나면 가로선을 이용해 옆 세로선으로 이동한 다음, 이동한 세로선에서 아래 방향으로 이동해야 한다.   

위의 그림에서 1번은 3번으로, 2번은 2번으로, 3번은 5번으로, 4번은 1번으로, 5번은 4번으로 도착하게 된다. 아래 두 그림은 1번과 2번이 어떻게 이동했는지 나타내는 그림이다.   

<p align="center">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/3.png" width="400">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/4.png" width="400">
</p>

사다리에 가로선을 추가해서, 사다리 게임의 결과를 조작하려고 한다. 이때, i번 세로선의 결과가 i번이 나와야 한다. 그렇게 하기 위해서 추가해야 하는 가로선 개수의 최솟값을 구하는 프로그램을 작성하시오.   

---

#### 입력

첫째 줄에 세로선의 개수 N, 가로선의 개수 M, 세로선마다 가로선을 놓을 수 있는 위치의 개수 H가 주어진다. (2 ≤ N ≤ 10, 1 ≤ H ≤ 30, 0 ≤ M ≤ (N-1)×H)   

둘째 줄부터 M개의 줄에는 가로선의 정보가 한 줄에 하나씩 주어진다.   

가로선의 정보는 두 정수 a과 b로 나타낸다. (1 ≤ a ≤ H, 1 ≤ b ≤ N-1) b번 세로선과 b+1번 세로선을 a번 점선 위치에서 연결했다는 의미이다.   

가장 위에 있는 점선의 번호는 1번이고, 아래로 내려갈 때마다 1이 증가한다. 세로선은 가장 왼쪽에 있는 것의 번호가 1번이고, 오른쪽으로 갈 때마다 1이 증가한다.   

입력으로 주어지는 가로선이 서로 연속하는 경우는 없다.   

---

#### 출력

i번 세로선의 결과가 i번이 나오도록 사다리 게임을 조작하려면, 추가해야 하는 가로선 개수의 최솟값을 출력한다. 만약, 정답이 3보다 큰 값이면 -1을 출력한다. 또, 불가능한 경우에도 -1을 출력한다.

---
#### 예제 입력 1

~~~
2 0 3
~~~

#### 예제 출력 1

~~~
2 1 3
1 1
~~~

#### 예제 입력 2

~~~
2 1 3
1 1
~~~

#### 예제 출력 2

~~~
1
~~~

#### 예제 입력 3

~~~
5 5 6
1 1
3 2
2 3
5 1
5 4
~~~

#### 예제 출력 3

~~~
3
~~~

#### 예제 입력 4

~~~
6 5 6
1 1
3 2
1 3
2 5
5 5
~~~

#### 예제 출력 4

~~~
3
~~~

#### 예제 입력 5

~~~
5 8 6
1 1
2 2
3 3
4 4
3 1
4 2
5 3
6 4
~~~

#### 예제 출력 5

~~~
-1
~~~

#### 예제 입력 6

~~~
5 12 6
1 1
1 3
2 2
2 4
3 1
3 3
4 2
4 4
5 1
5 3
6 2
6 4
~~~

#### 예제 출력 6

~~~
-1
~~~

#### 예제 입력 7

~~~
2 0 3
~~~

#### 예제 출력 7

~~~
2 1 3
1 1
~~~

---

#### 힌트

**예제 3, 예제 3 정답**
<p align="center">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/ex3.png" width="400">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/ans3.png" width="400">
</p>

**예제 7, 예제 7 정답**
<p align="center">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/ex7.png" width="400">
  <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15684/ans7.png" width="400">
</p>


출처 : https://www.acmicpc.net/problem/15684

---

### 문제풀이

이 문제를 풀기 위해서는 모든 경우의 수를 확인해야 합니다.   

그래서 이 문제는 완전 탐색 알고리즘으로 해결할 수 있습니다.   

대략적으로 경우의 수를 계산하면 270 * 269 * 268 = 19,464,840번의 연산이 필요합니다.   

거기에 사다리 타기가 정답인지 확인까지 해야 하기 때문에 대략 5억번의 연산이 필요하게 됩니다.   

그래서 pyhton으로 제출하면 시간 초과가 날 것으로 예상됐는데, 역시 시간 초과 판정을 받았습니다.   

그런데 동일한 로직으로 pypy3으로 제출하니 바로 통과했습니다.   

이 문제를 풀 때, 어려웠던 점은 정답 알고리즘을 떠올리는 것이 아니라 구현하는 것이었습니다.   

사다리 타기도 구현해야 하고, 사다리 타기가 정답인지 확인하는 것도 구현해야 하고, 완전 탐색 알고리즘도 구현해야 해서 까다로웠습니다.   

이 문제는 구현을 연습하는 데 좋은 문제였던 것 같고, 책정되어 있는 난이도보다 더 어려운 문제였습니다.

아래에 python으로만 통과하신 분의 풀이를 첨부했습니다.

---

#### 나의 풀이

~~~python
n, m, h = map(int, input().split())

# 사다리 정보를 저장할 리스트
ladder = [[0 for i in range(h+2)] for _ in range(n+2)]
answer = []
# 사다리 정보를 입력
for _ in range(m):
    a, b = map(int, input().split())
    ladder[b][a] = b+1
    ladder[b+1][a] = b
# 사다리의 정보가 정답인지 확인하는 함수
def chk_ladder():
    for i in range(1, n+1):
        if not is_same_end(i):
            return False
    return True
# 사다리 타기를 해서 출발점과 도착점이 같은지 확인하는 함수
def is_same_end(x):
    end = x
    for y in range(1, h + 1):
        if ladder[x][y]:
            x = ladder[x][y]
    return end == x
# 완전 탐색을 하는 함수를 재귀로 구현
def back_tracking(cnt, row, col):
    # 사다리 타기가 정답인 경우
    if chk_ladder():
        # 정답이 0 혹은 1인 경우 예외처리를 통해 바로 종료
        if 0 <= cnt <= 1:
            print(cnt)
            exit()
        answer.append(cnt)

    if cnt == 3:
        return
    # x축, y축 탐색을 while문으로 구현
    x = row
    y = col
    while x < n and y < h + 1:
        # 현재 확인하고 있는 지점에서 좌우의 선이 연결되지 않았는지 확인
        if ladder[x-1][y] != x and ladder[x+1][y] == 0:
            ladder[x][y] = x + 1
            ladder[x+1][y] = x
            back_tracking(cnt + 1, x, y)
            ladder[x][y] = 0
            ladder[x+1][y] = 0
        y += 1
        # 현재 y축을 모두 확인하면 다음 x축을 
        if y == h + 1:
            x += 1
            y = 1

back_tracking(0, 1, 1)

print(min(answer) if answer else -1)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/22587682

~~~python
def check(pos) :
    for i in range(len(pos[0])+1) :
        pre = i
        for j in range(len(pos)) :
            if i < len(pos[0]) and pos[j][i] :
                i += 1
            elif i > 0 and pos[j][i-1] :
                i -= 1
        if pre != i :
            return False
    return True

def dfs(pos, n, m, line) :
    if m <= n :
        return m
    if n == 4 :
        return 4
    if check(pos) :
        return n 
    odd = 0
    for i in line :
        if i%2 == 1 :
            odd += 1
    if odd > 3-n :
        return 4
    for i in range(len(pos)):
        for j in range(len(pos[0])) :
            if pos[i][j] :
                continue
            if j+1 < len(pos[i]) and pos[i][j+1] :
                continue
            if j-1 > 0 and pos[i][j-1] :
                continue
            pos[i][j] = True
            line[j] += 1
            m = min(m, dfs(pos, n+1, m, line))
            pos[i][j] = False
            line[j] -= 1
    return m
    
n, m, p = map(int, input().split())
pos = [[False for j in range(n-1)] for i in range(p)]
line = [0 for i in range(n-1)]

for i in range(m) :
    a = list(map(int, input().split()))
    b = a[1] - 1
    a = a[0] - 1
    pos[a][b] = True
    line[b] += 1

answer = dfs(pos, 0, 4, line)
if answer == 4 :
    answer = -1

print(answer)
~~~
