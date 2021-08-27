# 색종이 만들기 (Silver 3)

### 문제 설명

아래 <그림 1>과 같이 여러개의 정사각형칸들로 이루어진 정사각형 모양의 종이가 주어져 있고, 각 정사각형들은 하얀색으로 칠해져 있거나 파란색으로 칠해져 있다. 주어진 종이를 일정한 규칙에 따라 잘라서 다양한 크기를 가진 정사각형 모양의 하얀색 또는 파란색 색종이를 만들려고 한다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/bwxBxc7ghGOedQfiT3p94KYj1y9aLR.png">

전체 종이의 크기가 N×N(N=2k, k는 1 이상 7 이하의 자연수) 이라면 종이를 자르는 규칙은 다음과 같다.   

전체 종이가 모두 같은 색으로 칠해져 있지 않으면 가로와 세로로 중간 부분을 잘라서 <그림 2>의 I, II, III, IV와 같이 똑같은 크기의 네 개의 N/2 × N/2색종이로 나눈다. 나누어진 종이 I, II, III, IV 각각에 대해서도 앞에서와 마찬가지로 모두 같은 색으로 칠해져 있지 않으면 같은 방법으로 똑같은 크기의 네 개의 색종이로 나눈다. 이와 같은 과정을 잘라진 종이가 모두 하얀색 또는 모두 파란색으로 칠해져 있거나, 하나의 정사각형 칸이 되어 더 이상 자를 수 없을 때까지 반복한다.   

위와 같은 규칙에 따라 잘랐을 때 <그림 3>은 <그림 1>의 종이를 처음 나눈 후의 상태를, <그림 4>는 두 번째 나눈 후의 상태를, <그림 5>는 최종적으로 만들어진 다양한 크기의 9장의 하얀색 색종이와 7장의 파란색 색종이를 보여주고 있다.   

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/VHJpKWQDv.png">

입력으로 주어진 종이의 한 변의 길이 N과 각 정사각형칸의 색(하얀색 또는 파란색)이 주어질 때 잘라진 하얀색 색종이와 파란색 색종이의 개수를 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에는 전체 종이의 한 변의 길이 N이 주어져 있다. N은 2, 4, 8, 16, 32, 64, 128 중 하나이다. 색종이의 각 가로줄의 정사각형칸들의 색이 윗줄부터 차례로 둘째 줄부터 마지막 줄까지 주어진다. 하얀색으로 칠해진 칸은 0, 파란색으로 칠해진 칸은 1로 주어지며, 각 숫자 사이에는 빈칸이 하나씩 있다.

---

#### 출력

첫째 줄에는 잘라진 햐얀색 색종이의 개수를 출력하고, 둘째 줄에는 파란색 색종이의 개수를 출력한다.

---
#### 예제 입력 1

~~~
8
1 1 0 0 0 0 1 1
1 1 0 0 0 0 1 1
0 0 0 0 1 1 0 0
0 0 0 0 1 1 0 0
1 0 0 0 1 1 1 1
0 1 0 0 1 1 1 1
0 0 1 1 1 1 1 1
0 0 1 1 1 1 1 1
~~~

#### 예제 출력 1

~~~
9
7
~~~

[출처](https://www.acmicpc.net/problem/2630)

---

### 문제풀이

이번 문제는 분할 정복 문제입니다.   

색종이의 색을 확인하고, 전부 일치하지 않는 경우는 4등분으로 접어서 다시 확인합니다.   

색종이의 크기가 1이 될때까지 확인하고 접는 것을 반복합니다.   

저는 색종이의 색을 확인하는 함수와 색종이를 4등분으로 나누는 함수 2가지 구분해서 문제를 해결했습니다.   

다른 분들의 풀이를 보니 리스트를 나누지 않고 문제를 해결한 것을 알 수 있는데, 저도 다음에 그런 방식으로 풀어봐야 겠습니다.

---

#### 나의 풀이

~~~python
n = int(input())
paper = [list(map(int, input().split())) for _ in range(n)]
answer = [0, 0]
# 색종이의 색을 확인하는 함수
def is_same_color(p):
    s = sum(map(sum, p))
    return (True, int(s > 0)) if s == 0 or s == len(p)**2 else (False, 0)
# 색종이를 4등분 하는 함수
def split_square(p):
    h = len(p) // 2
    squares = []
    temp = []
    for i in p[:h]:
        temp.append(i[:h])
    squares.append(temp)

    temp = []
    for i in p[:h]:
        temp.append(i[h:])
    squares.append(temp)

    temp = []
    for i in p[h:]:
        temp.append(i[:h])
    squares.append(temp)

    temp = []
    for i in p[h:]:
        temp.append(i[h:])
    squares.append(temp)

    return squares
# 재귀로 분할 정복하는 함수
def recursive(p):
    same, color = is_same_color(p)

    if same or len(p) == 1:
        answer[color] += 1
        return
    
    squares = split_square(p)

    for square in squares:
        recursive(square)

recursive(paper)
print(*answer, sep='\n')
~~~

#### 다른 사람의 풀이

~~~python
import sys
r=sys.stdin.readline

def c(n,x=0,y=0):
    global z,o
    d0=1-d[x][y]
    for i in range(n):
        if d0 in d[x+i][y:y+n]:
            c(n//2,x,y)
            c(n//2,x+n//2,y)
            c(n//2,x,y+n//2)
            c(n//2,x+n//2,y+n//2)
            return
    if d0:
        z+=1
    else:
        o+=1

n = int(r())
d = [list(map(int,r().split())) for _ in range(n)]

z=o=0
c(n)
print(z,o)
~~~
