# Z (Silver 1)

### 문제 설명

한수는 크기가 2N × 2N인 2차원 배열을 Z모양으로 탐색하려고 한다. 예를 들어, 2×2배열을 왼쪽 위칸, 오른쪽 위칸, 왼쪽 아래칸, 오른쪽 아래칸 순서대로 방문하면 Z모양이다.

<img src="https://upload.acmicpc.net/21c73b56-5a91-43aa-b71f-9b74925c0adc/-/preview/" width=100>

N > 1인 경우, 배열을 크기가 2N-1 × 2N-1로 4등분 한 후에 재귀적으로 순서대로 방문한다.   

다음 예는 22 × 22 크기의 배열을 방문한 순서이다.   

<img src="https://upload.acmicpc.net/adc7cfae-e84d-4d5c-af8e-ee011f8fff8f/-/preview/" width=250>

N이 주어졌을 때, r행 c열을 몇 번째로 방문하는지 출력하는 프로그램을 작성하시오.   

다음은 N=3일 때의 예이다.   

<img src="https://upload.acmicpc.net/d3e84bb7-9424-4764-ad3a-811e7fcbd53f/-/preview/" width=533>

---

#### 입력

첫째 줄에 정수 N, r, c가 주어진다.

---

#### 출력

r행 c열을 몇 번째로 방문했는지 출력한다.

---

#### 제한

* 1 ≤ N ≤ 15
* 0 ≤ r, c < 2^N

---

#### 예제 입력 1

~~~
2 3 1
~~~

#### 예제 출력 1

~~~
11
~~~

#### 예제 입력 2

~~~
3 7 7
~~~

#### 예제 출력 2

~~~
63
~~~

[출처](https://www.acmicpc.net/problem/1074)

---

### 문제풀이

이번 문제는 재귀를 사용하는 분할 정복 문제입니다.   

2^N * 2^N의 사각형에서 특정 행렬의 숫자를 맞추는 문제입니다.   

행과 열의 값을 바탕으로 정답의 범위를 줄여나가면 정답을 맞출 수 있습니다.   

저는 어렵게 풀었는데 다른 분이 깔끔하게 푼 풀이가 있어서 아래에 첨부했습니다.

---

#### 나의 풀이

~~~python
n, r, c = map(int, input().split())

def recursive(s, e, n):
    global r, c
    if n == -1:
        print(s)
        return
    h = 2**n
    if r >= h:
        s += 2*(h**2)
        r -= h
    else:
        e -= 2*(h**2)
    if c >= h:
        s += h**2
        c -= h
    else:
        e -= h**2
    recursive(s, e, n-1)

recursive(0, 2**(n*2)-1, n-1)
~~~

---

#### 다른 사람의 풀이

~~~python
n, r, c = map(int, input().split())
x = 0

for i in range(n):
    dr = 2 << (2 * i)
    dc = 1 << (2 * i)

    if r & (1 << i):
        x += dr
    if c & (1 << i):
        x += dc
print(x)
~~~
