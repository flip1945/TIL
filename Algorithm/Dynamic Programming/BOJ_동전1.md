# 동전1(Silver 1)

### 문제

n가지 종류의 동전이 있다. 각각의 동전이 나타내는 가치는 다르다. 이 동전을 적당히 사용해서, 그 가치의 합이 k원이 되도록 하고 싶다. 그 경우의 수를 구하시오. 각각의 동전은 몇 개라도 사용할 수 있다.

사용한 동전의 구성이 같은데, 순서만 다른 것은 같은 경우이다.

---

#### 입력

첫째 줄에 n, k가 주어진다. (1 ≤ n ≤ 100, 1 ≤ k ≤ 10,000) 다음 n개의 줄에는 각각의 동전의 가치가 주어진다. 동전의 가치는 100,000보다 작거나 같은 자연수이다.

---

#### 출력

첫째 줄에 경우의 수를 출력한다. 경우의 수는 2^31보다 작다.

---

#### 예제 입력 1
~~~
3 10
1
2
5
~~~

#### 예제 출력 1
~~~
10
~~~

[출처](https://www.acmicpc.net/problem/2293)

---

### 문제풀이

이번 문제는 Dynamic Programming 문제입니다.   

기본적인 문제라고 생각하는데, 푸는 방법을 떠올리는 데 조금 어려웠습니다.   

코인 테이블을 만들고, k원이 될 때까지 계속 만들수 있는 가짓수 값을 갱신해주면 되는 문제입니다.   

저는 동전이 있을 때, dp 테이블에 값이 존재할 때 등 여러 조건을 달았는 데 다른 분들의 풀이를 보니 더 쉽게 푸는 방법이 있었습니다.   

dp문제를 풀 때마다 어려움을 느끼는 것 같아서 더 연습을 해야 겠습니다.   

---

#### 나의 풀이

~~~python
n, k = map(int, input().split())
dp = [0] * (k+1)

for _ in range(n):
    coin = int(input())
    for i in range(k+1):
        if dp[i]:
            if i+coin <= k:
                dp[i+coin] += dp[i]
        elif not i % coin:
            dp[i] += 1
print(dp[k])
~~~

---

#### 다른 사람의 풀이

~~~python
n, k = map(int, input().split())
dp = [0] * (k+1)

for _ in range(n):
    coin = int(input())
    for i in range(k+1):
        if dp[i]:
            if i+coin <= k:
                dp[i+coin] += dp[i]
        elif not i % coin:
            dp[i] += 1
print(dp[k])
~~~
