# 숫자 정사각형 (Silver 3)

### 문제 설명

N\*M크기의 직사각형이 있다. 각 칸은 한 자리 숫자가 적혀 있다. 이 직사각형에서 꼭짓점에 쓰여 있는 수가 모두 같은 가장 큰 정사각형을 찾는 프로그램을 작성하시오. 이때, 정사각형은 행 또는 열에 평행해야 한다.

---

#### 입력

첫째 줄에 N과 M이 주어진다. N과 M은 50보다 작거나 같은 자연수이다. 둘째 줄부터 N개의 줄에 수가 주어진다.

---

#### 출력

첫째 줄에 정답 정사각형의 크기를 출력한다.

---
#### 예제 입력 1

~~~
3 5
42101
22100
22101
~~~

#### 예제 출력 1

~~~
9
~~~

출처 : https://www.acmicpc.net/problem/1051

---

### 문제풀이

이번 문제는 완전 탐색 문제입니다.   

가장 큰 정사각형의 크기를 구하는 문제입니다.

4개의 꼭지점의 숫자가 같은 경우에만 인정이 되는데, 이때 여러 정사각형 중에 가장 큰 정사각형의 크기를 출력하면 됩니다.

이 문제는 n, m의 크기가 50 이하의 숫자로 매우 작기 때문에 완전 탐색 알고리즘을 이용하면 문제를 해결할 수 있습니다.

---

#### 나의 풀이

~~~python
n, m = map(int, input().split())
square = [list(map(int, input())) for i in range(n)]

def max_square(i, j):
    num = square[i][j]
    result = 0
    for size in range(min(n, m)):
        if i + size < n and j + size < m and square[i+size][j] == num and square[i][j+size] == num and square[i+size][j+size] == num:
            result = (size + 1) ** 2
    return result

answer = 0
for row in range(n):
    for col in range(m):
        answer = max(answer, max_square(row, col))

print(answer)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/16132863

~~~python
n,m=map(int,input().split())
a=[input() for _ in range(n)]
M=0
for i in range(n):
  for j in range(m):
    for k in range(min(n-i,m-j)):
      if a[i][j]==a[i][j+k]==a[i+k][j]==a[i+k][j+k] and M<(k+1)**2:
        M=(k+1)**2
print(M)
~~~
