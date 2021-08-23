# 듣보잡 (Silver 4)

### 문제

김진영이 듣도 못한 사람의 명단과, 보도 못한 사람의 명단이 주어질 때, 듣도 보도 못한 사람의 명단을 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 듣도 못한 사람의 수 N, 보도 못한 사람의 수 M이 주어진다. 이어서 둘째 줄부터 N개의 줄에 걸쳐 듣도 못한 사람의 이름과, N+2째 줄부터 보도 못한 사람의 이름이 순서대로 주어진다. 이름은 띄어쓰기 없이 영어 소문자로만 이루어지며, 그 길이는 20 이하이다. N, M은 500,000 이하의 자연수이다.   

듣도 못한 사람의 명단에는 중복되는 이름이 없으며, 보도 못한 사람의 명단도 마찬가지이다.

---

#### 출력

듣보잡의 수와 그 명단을 사전순으로 출력한다.

---

#### 예제 입력 1
~~~
3 4
ohhenrie
charlie
baesangwook
obama
baesangwook
ohhenrie
clinton
~~~

#### 예제 출력 1
~~~
2
baesangwook
ohhenrie
~~~

[출처](https://www.acmicpc.net/problem/1764)

---

### 문제풀이

이 문제는 hashmap 자료 구조를 사용해서 푸는 문제입니다.   

2번에 걸쳐서 이름들이 주어지는 데, 이중에 겹치는 이름들만 정렬해서 제출하는 문제입니다.   

저는 이 문제를 보자마자 defaultdict를 사용해야 겠다고 생각했습니다.   

defaultdict를 사용하면 같은 이름이 몇 번 등장했는 지 쉽게 파악할 수 있습니다.   

저는 defaultdict를 사용해서 문제를 잘 풀었는 데, 다른 분들이 set을 이용해서 더 간단하게 푼 것을 보고 놀랐습니다.   

다음에 비슷한 문제를 풀게 된다면 다음에는 set을 이용한 풀이로 풀어봐야 겠습니다.

---

#### 나의 풀이

~~~python
from collections import defaultdict
import sys

n, m = map(int, input().split())
names = defaultdict(int)

for i in range(n + m):
    names[sys.stdin.readline().rstrip()] += 1

names = sorted([name for name in names.keys() if names[name] > 1])

print(len(names), *names, sep='\n')
~~~

---

#### 다른 사람의 풀이

~~~python
import sys
n, m = map(int, input().split())
lst = sys.stdin.read().split()
r = sorted(set(lst[:n]) & set(lst[n:]))
print(len(r), *r, sep='\n')
~~~
