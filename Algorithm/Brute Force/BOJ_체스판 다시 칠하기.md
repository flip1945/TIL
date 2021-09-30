# 체스판 다시 칠하기 (Silver 5)

### 문제 설명

지민이는 자신의 저택에서 MN개의 단위 정사각형으로 나누어져 있는 M\*N 크기의 보드를 찾았다. 어떤 정사각형은 검은색으로 칠해져 있고, 나머지는 흰색으로 칠해져 있다. 지민이는 이 보드를 잘라서 8\*8 크기의 체스판으로 만들려고 한다.   

체스판은 검은색과 흰색이 번갈아서 칠해져 있어야 한다. 구체적으로, 각 칸이 검은색과 흰색 중 하나로 색칠되어 있고, 변을 공유하는 두 개의 사각형은 다른 색으로 칠해져 있어야 한다. 따라서 이 정의를 따르면 체스판을 색칠하는 경우는 두 가지뿐이다. 하나는 맨 왼쪽 위 칸이 흰색인 경우, 하나는 검은색인 경우이다.   

보드가 체스판처럼 칠해져 있다는 보장이 없어서, 지민이는 8\*8 크기의 체스판으로 잘라낸 후에 몇 개의 정사각형을 다시 칠해야겠다고 생각했다. 당연히 8\*8 크기는 아무데서나 골라도 된다. 지민이가 다시 칠해야 하는 정사각형의 최소 개수를 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N과 M이 주어진다. N과 M은 8보다 크거나 같고, 50보다 작거나 같은 자연수이다. 둘째 줄부터 N개의 줄에는 보드의 각 행의 상태가 주어진다. B는 검은색이며, W는 흰색이다.

---

#### 출력

첫째 줄에 지민이가 다시 칠해야 하는 정사각형 개수의 최솟값을 출력한다.

---
#### 예제 입력 1

~~~
8 8
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBBBWBW
WBWBWBWB
BWBWBWBW
WBWBWBWB
BWBWBWBW
~~~

#### 예제 출력 1

~~~
1
~~~

#### 예제 입력 2

~~~
10 13
BBBBBBBBWBWBW
BBBBBBBBBWBWB
BBBBBBBBWBWBW
BBBBBBBBBWBWB
BBBBBBBBWBWBW
BBBBBBBBBWBWB
BBBBBBBBWBWBW
BBBBBBBBBWBWB
WWWWWWWWWWBWB
WWWWWWWWWWBWB
~~~

#### 예제 출력 2

~~~
12
~~~

출처 : https://www.acmicpc.net/problem/1018

---

### 문제풀이

이번 문제는 완전 탐색문제입니다.   

전체 체스판의 크기가 50x50 으로 매우 작기 때문에 올바른 체스판과 얼마나 차이가 있는지 전체 체스판을 모두 확인하면 됩니다.   

저는 8x8 체스판을 리스트로 만들어서 문제를 풀었는데, 다른 분의 풀이를 보니 더 쉽게 풀어서 재밌었습니다.   

제가 문제를 풀 때도 더 좋은 방법이 있을 것 같았는데, 명확히 알게돼서 좋았습니다.

---

#### 나의 풀이

~~~python
n, m = map(int, input().split())
board = [list(input()) for _ in range(n)]
chess = []
answer = int(10e9)

for i in range(4):
    chess.append(['B', 'W', 'B', 'W', 'B', 'W', 'B', 'W'])
    chess.append(['W', 'B', 'W', 'B', 'W', 'B', 'W', 'B'])
chess.append(['B', 'W', 'B', 'W', 'B', 'W', 'B', 'W'])

def get_repaint_cnt(c, x, y):
    cnt = 0
    for row in range(8):
        for col in range(8):
            if board[row + x][col + y] != c[row][col]:
                cnt += 1
    return cnt

for i in range(n - 7):
    for j in range(m - 7):
        answer = min(get_repaint_cnt(chess[1:], i, j), get_repaint_cnt(chess[:-1], i, j), answer)

print(answer)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/18861954

~~~python
m,n=map(int,input().split())
l=[input() for i in range(m)]
M=64
for i in range(m-7):
    for j in range(n-7):
        c=0
        for k in range(8):
            for t in range(8):
                if l[i+k][j+t]=='WB'[(k+t)%2]:
                    c+=1
        M=min(c,64-c,M)
print(M)
~~~
