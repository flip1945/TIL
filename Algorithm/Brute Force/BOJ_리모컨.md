# 리모컨 (Gold 5)

### 문제 설명

수빈이는 TV를 보고 있다. 수빈이는 채널을 돌리려고 했지만, 버튼을 너무 세게 누르는 바람에, 일부 숫자 버튼이 고장났다.

리모컨에는 버튼이 0부터 9까지 숫자, +와 -가 있다. +를 누르면 현재 보고있는 채널에서 +1된 채널로 이동하고, -를 누르면 -1된 채널로 이동한다. 채널 0에서 -를 누른 경우에는 채널이 변하지 않고, 채널은 무한대 만큼 있다.

수빈이가 지금 이동하려고 하는 채널은 N이다. 어떤 버튼이 고장났는지 주어졌을 때, 채널 N으로 이동하기 위해서 버튼을 최소 몇 번 눌러야하는지 구하는 프로그램을 작성하시오. 

수빈이가 지금 보고 있는 채널은 100번이다.

---

#### 입력

첫째 줄에 수빈이가 이동하려고 하는 채널 N (0 ≤ N ≤ 500,000)이 주어진다.  둘째 줄에는 고장난 버튼의 개수 M (0 ≤ M ≤ 10)이 주어진다. 고장난 버튼이 있는 경우에는 셋째 줄에는 고장난 버튼이 주어지며, 같은 버튼이 여러 번 주어지는 경우는 없다.

---

#### 출력

첫째 줄에 채널 N으로 이동하기 위해 버튼을 최소 몇 번 눌러야 하는지를 출력한다.

---
#### 예제 입력 1

~~~
5457
3
6 7 8
~~~

#### 예제 출력 1

~~~
6
~~~

#### 예제 입력 2

~~~
100
5
0 1 2 3 4
~~~

#### 예제 출력 2

~~~
0
~~~

#### 예제 입력 3

~~~
500000
8
0 2 3 4 6 7 8 9
~~~

#### 예제 출력 3

~~~
11117
~~~

---

#### 힌트

5455++ 또는 5459--

출처 : https://www.acmicpc.net/problem/1107

---

### 문제풀이

이번 문제는 완전 탐색 문제입니다.

저는 중복 순열을 모두 구하고자 product 함수를 사용했는데,

잘 생각해보니 n의 크기가 100만 정도이므로 반복문을 이용해도 문제를 풀 수 있을 것 같습니다.

더 쉬운 풀이 방법이 있었는데 괜히 어렵게 풀어서 에러가 많이 발생했습니다.

이 문제는 코너 케이스를 모두 생각하면서 문제를 풀어야 정답을 맞출 수 있습니다.

m이 0인 경우에서 예외처리를 해야하고, 현재 채널의 길이보다

버튼을 더 적게 누르거나 많이 눌렀을 때도 모두 확인해야 합니다.

저는 어렵게 풀었지만 반복문을 이용해 더 쉽게 푼 분의 풀이를 아래에 첨부했습니다.

---

#### 나의 풀이

~~~python
from itertools import product

n = int(input())
n_len = len(str(n))
a = int(input())
m = list(map(str, range(10)))
if a != 0:
    m = set(range(10)) - set(map(int, input().split()))
min_abs = int(10e9)
add = 0

for i in [-1, 0, 1]:
    for j in product(map(str, m), repeat=n_len + i):
        if j != () and min_abs > abs(n - int(''.join(j))):
            min_abs = min(min_abs, abs(n - int(''.join(j))))
            add = i

print(min(abs(100 - n), min_abs + n_len + add))
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/29357426

~~~python
target = int(input())
ans = abs(100 - target)
M = int(input())
if M:
    broken = set(input().split())
else:
    broken = set()

for num in range(1000001):
    for N in str(num):
        if N in broken:
            break
    else:
        ans = min(ans, len(str(num)) + abs(num - target))

print(ans)
~~~
