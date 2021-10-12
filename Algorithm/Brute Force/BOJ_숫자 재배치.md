# 숫자 재배치 (Silver 1)

### 문제 설명

두 정수 A와 B가 있을 때, A에 포함된 숫자의 순서를 섞어서 새로운 수 C를 만들려고 한다. 즉, C는 A의 순열 중 하나가 되어야 한다. 

가능한 C 중에서 B보다 작으면서, 가장 큰 값을 구해보자. C는 0으로 시작하면 안 된다.

---

#### 입력

첫째 줄에 두 정수 A와 B가 주어진다.

---

#### 출력

B보다 작은 C중에서 가장 큰 값을 출력한다. 그러한 C가 없는 경우에는 -1을 출력한다.

---

#### 제한

* 1 ≤ A, B < 10^9

---
#### 예제 입력 1

~~~
1234 3456
~~~

#### 예제 출력 1

~~~
3421
~~~

#### 예제 입력 2

~~~
1000 5
~~~

#### 예제 출력 2

~~~
-1
~~~

#### 예제 입력 3

~~~
789 123
~~~

#### 예제 출력 3

~~~
-1
~~~

---

출처 : https://www.acmicpc.net/problem/16943

---

### 문제풀이

이번 문제는 완전 탐색 문제입니다.   

순열을 구하는 문제로 itertools의 permutations 함수를 사용해 문제를 풀 수 있습니다.

이 문제에서 유의할 점은 a의 숫자를 모두 사용해야 한다는 점, 그리고 순열의 앞자리가 0인 경우 정답으로 인정하지 않는다는 점입니다.

이 점을 유의하면서 문제를 풀면 쉽게 정답을 맞출 수 있습니다.

---

#### 나의 풀이

~~~python
from itertools import permutations

a, b = map(int, input().split())
c = -1

for i in permutations(str(a), len(str(a))):
    if i[0] == '0':
        continue
    num = int(''.join(i))

    if num < b:
        c = max(c, num)

print(c)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/11859471

~~~python
from itertools import permutations
ans = -1
a, b = input().split()
for x in permutations(a):
    if int("".join(x)) <= int(b) and x[0] != '0':
        ans = max(int("".join(x)), ans)
print(ans)
~~~
