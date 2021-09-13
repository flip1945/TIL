# 구간 합 구하기4 (Silver 3)

### 문제 설명

수 N개가 주어졌을 때, i번째 수부터 j번째 수까지 합을 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 수의 개수 N과 합을 구해야 하는 횟수 M이 주어진다. 둘째 줄에는 N개의 수가 주어진다. 수는 1,000보다 작거나 같은 자연수이다. 셋째 줄부터 M개의 줄에는 합을 구해야 하는 구간 i와 j가 주어진다.

---

#### 출력

총 M개의 줄에 입력으로 주어진 i번째 수부터 j번째 수까지 합을 출력한다.

---

#### 제한

* 1 ≤ N ≤ 100,000
* 1 ≤ M ≤ 100,000
* 1 ≤ i ≤ j ≤ N

---

#### 예제 입력 1

~~~
5 3
5 4 3 2 1
1 3
2 4
5 5
~~~

#### 예제 출력 1

~~~
12
9
1
~~~

출처 : https://www.acmicpc.net/problem/1074

---

### 문제풀이

이번 문제는 누적합을 구하는 문제입니다.   

랜덤으로 주어진 숫자 리스트에서 특정한 구간의 합을 구해야 하는 문제입니다.   

이 때 구간합을 구할 때, sum 함수를 사용하면 구간 별로 모든 요소를 확인하기 때문에 구간별로 O(n)시간이 걸립니다.   

이 문제에서 n과 m의 최대 값이 10만으로 설정되어 있는데,   

sum을 이용해 구간합을 구하면 O(nm)의 시간복잡도가 소요됩니다.   

이 때는 10만 * 10만의 연산으로 시간 초과 판정을 받을 수 밖에 없기 때문에 이 문제를 해결하기 위해서는 누적합 알고리즘을 사용해야 합니다.   

저는 누적합을 이용해서 문제를 풀었고 풀이는 아래에 있습니다.

---

#### 나의 풀이

~~~python
import sys
_, m = map(int, input().split())
n = list(map(int, input().split()))

for i in range(1, len(n)):
    n[i] += n[i-1]

for _ in range(m):
    i, j = map(int, sys.stdin.readline().split())
    if i == 1:
        print(n[j-1])
    else:
        print(n[j-1] - n[i-2])
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/32507048

~~~python
import sys
P=lambda:map(int,sys.stdin.readline().split())
n,m=P()
l=[0,*P()]
for i in range(n+1):l[i]+=l[i-1]
for _ in" "*m:i,j=P();print(l[j]-l[i-1])
~~~
