# 개똥벌레 (Gold 5)

### 문제

개똥벌레 한 마리가 장애물(석순과 종유석)로 가득찬 동굴에 들어갔다. 동굴의 길이는 N미터이고, 높이는 H미터이다. (N은 짝수) 첫 번째 장애물은 항상 석순이고, 그 다음에는 종유석과 석순이 번갈아가면서 등장한다.   

아래 그림은 길이가 14미터이고 높이가 5미터인 동굴이다. (예제 그림)   

<p align="center">
  <img src="https://upload.acmicpc.net/c6fd496d-ccf5-4f9d-a06e-32b121fc6a82/-/preview/" width=270>
</p>

이 개똥벌레는 장애물을 피하지 않는다. 자신이 지나갈 구간을 정한 다음 일직선으로 지나가면서 만나는 모든 장애물을 파괴한다.   

위의 그림에서 4번째 구간으로 개똥벌레가 날아간다면 파괴해야하는 장애물의 수는 총 여덟개이다. (4번째 구간은 길이가 3인 석순과 길이가 4인 석순의 중간지점을 말한다)   

<p align="center">
  <img src="https://upload.acmicpc.net/bfcbb94f-0e15-4ff9-b2ef-43e07c7ee503/-/preview/" width=270>
</p>

하지만, 첫 번째 구간이나 다섯 번째 구간으로 날아간다면 개똥벌레는 장애물 일곱개만 파괴하면 된다.   

동굴의 크기와 높이, 모든 장애물의 크기가 주어진다. 이때, 개똥벌레가 파괴해야하는 장애물의 최솟값과 그러한 구간이 총 몇 개 있는지 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N과 H가 주어진다. N은 항상 짝수이다. (2 ≤ N ≤ 200,000, 2 ≤ H ≤ 500,000)   

다음 N개 줄에는 장애물의 크기가 순서대로 주어진다. 장애물의 크기는 H보다 작은 양수이다.

---

#### 출력

첫째 줄에 개똥벌레가 파괴해야 하는 장애물의 최솟값과 그러한 구간의 수를 공백으로 구분하여 출력한다.

---

#### 예제 입력 1
~~~
6 7
1
5
3
3
5
1
~~~

#### 예제 출력 1
~~~
2 3
~~~

#### 예제 입력 2
~~~
14 5
1
3
4
2
2
4
3
4
3
3
3
2
3
3
~~~

#### 예제 출력 2
~~~
7 2
~~~

[출처](https://www.acmicpc.net/problem/3020)

---

### 문제풀이

이 문제는 구간 합 구하기 알고리즘을 사용해 푸는 문제입니다.   

혹은 정렬을 통해서 이분 탐색으로도 문제를 해결할 수 있습니다.   

저는 구간 합 구하기 방식을 이용해 문제를 풀었습니다.   

먼저 석순들의 높이들을 입력받았을 때, 가장 뒤에서부터 높이 별로 구간 합을 구해주면

각 높이 별로 개똥벌레가 지나야할 장애물의 개수를 저장할 수 있습니다.   

반대로 종유석의 장애물 개수를 저장해주고, 둘을 더하면 모든 높이의 장애물 개수를 알 수 있습니다.

---

#### 나의 풀이

1. 정답으로 제출한 풀이

~~~python
import sys
n, h = map(int, input().split())
low = [0] * (h+1)
high = [0] * (h+1)
dp = [0] * (h+2)

for i in range(n):
    if i % 2:  # 종유석 위에서 아래
        high[h - int(sys.stdin.readline().rstrip()) + 1] += 1
    else:  # 석순 아래에서 위
        low[int(sys.stdin.readline().rstrip())] += 1

for i in range(1, h+1):
    high[i] += high[i-1]

for i in range(h-1, 0, -1):
    low[i] += low[i+1]

for i in range(h+1):
    dp[i] = low[i] + high[i]

m = min(dp[1:h+1])
print(m, dp[1:h+1].count(m))
~~~

2. 제출 후 다듬은 풀이

~~~python
input = __import__('sys').stdin.readline
n, h = map(int, input().split())
low = [0] * h
high = [0] * h

for i in range(n):
    if i % 2:
        high[h-int(input())] += 1
    else:
        low[int(input())-1] += 1

for i in range(1, h):
    high[i] += high[i-1]
for i in range(h-1, 0, -1):
    low[i-1] += low[i]
    
for i in range(h):
    low[i] += high[i]

m = min(low)
print(m, low.count(m))
~~~

---

#### 다른 사람의 풀이

~~~python
input = __import__('sys').stdin.readline
n, h = map(int,input().split())
bot = [0]*h
top = [0]*h
for i in range(n//2):
    bot[int(input())-1]+= 1
    top[-int(input())]+= 1
for i in range(h-1):
    bot[~i-1]+= bot[~i]
    top[i+1]+= top[i]
for i in range(h): bot[i]+= top[i]
m = min(bot)
print(m, bot.count(m))
~~~
