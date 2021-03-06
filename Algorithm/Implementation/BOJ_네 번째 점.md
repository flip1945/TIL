# 네 번째 점 (Bronze 3)

### 문제 설명

세 점이 주어졌을 때, 축에 평행한 직사각형을 만들기 위해서 필요한 네 번째 점을 찾는 프로그램을 작성하시오.

---

#### 입력

세 점의 좌표가 한 줄에 하나씩 주어진다. 좌표는 1보다 크거나 같고, 1000보다 작거나 같은 정수이다.

---

#### 출력

직사각형의 네 번째 점의 좌표를 출력한다.

---

#### 예제 입력 1

~~~
5 5
5 7
7 5
~~~

#### 예제 출력 1

~~~
7 7
~~~

#### 예제 입력 2

~~~
30 20
10 10
10 20
~~~

#### 예제 출력 2

~~~
30 10
~~~

출처 : https://www.acmicpc.net/problem/3009

---

### 문제풀이

이번 문제는 단순한 구현 문제입니다.

원래 이렇게 쉬운 문제는 풀이를 기록하지 않는데, 이 문제를 기록하게 된 이유가 있습니다.

이 문제는 프로그래머스에서 코딩 테스트를 볼 때, 시험 보기 전 모의 테스트에 항상 나오는 문제입니다.

그 기억 때문에 이 문제를 풀게 됐습니다.

이 문제의 풀이는 간단합니다.

xy좌표상에서 점 3개가 찍혀있을 때, 직사각형을 만들기 위해서 필요한 나머지 점 1개의 위치를 찾는 문제입니다.

3개의 점 좌표 중에서 x 축에서 중복되지 않는 값과 y 축에서 중복되지 않는 값을 1개씩 찾아서 출력하면 정답을 맞출 수 있습니다.

저는 python의 Counter 클래스를 사용했는데, 다른 분의 풀이를 보니 XOR 연산으로 쉽게 푼 것을 보고 감탄했습니다.

XOR 연산의 새로운 사용법을 알게돼서 좋았습니다.

---

#### 나의 풀이

~~~python
from collections import Counter

n = [[x, y] for x, y in [input().split() for _ in range(3)]]
a = Counter([i[0] for i in n])
b = Counter([i[1] for i in n])

print(a.most_common(2)[1][0], b.most_common(2)[1][0])
~~~

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/14456650

~~~python
x=0; y=0
for _ in range(3):
	a,b = map(int, input().split())
	x ^= a; y ^= b
print(x, y)
~~~
