# 합 구하기 (Silver 3)

### 문제
N개의 수 A1, A2, ..., AN이 입력으로 주어진다. 총 M개의 구간 i, j가 주어졌을 때, i번째 수부터 j번째 수까지 합을 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 수의 개수 N이 주어진다. (1 ≤ N ≤ 100,000) 둘째 줄에는 A1, A2, ..., AN이 주어진다. (-1,000 ≤ Ai ≤ 1,000) 셋째 줄에는 구간의 개수 M이 주어진다. (1 ≤ M ≤ 100,000) 넷째 줄부터 M개의 줄에는 각 구간을 나타내는 i와 j가 주어진다. (1 ≤ i ≤ j ≤ N)

---

#### 출력

총 M개의 줄에 걸쳐 입력으로 주어진 구간의 합을 출력한다.

출처 : https://www.acmicpc.net/problem/11441

---

### 문제풀이

이번 문제는 누적합 문제입니다.

1차원 배열을 이용하면 되고 구간별 합을 구할 때 누적합 배열을 이용하면 쉽게 문제를 풀 수 있습니다.

---

#### 나의 풀이

~~~python
import sys
n = int(input())
a = list(map(int, input().split()))
prefix_sum = [0]
for num in a:
    prefix_sum.append(prefix_sum[-1] + num)

for _ in range(int(input())):
    i, j = map(int, sys.stdin.readline().split())
    print(prefix_sum[j] - prefix_sum[i-1])
~~~
