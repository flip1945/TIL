# 블랙잭(Bronze 2)

### 문제 설명

N개의 정수로 이루어진 배열 A가 주어진다. 이때, 배열에 들어있는 정수의 순서를 적절히 바꿔서 다음 식의 최댓값을 구하는 프로그램을 작성하시오   

|A\[0] - A\[1]| + |A\[1] - A\[2]| + ... + |A\[N-2] - A\[N-1]|    

---

#### 입력

첫째 줄에 N (3 ≤ N ≤ 8)이 주어진다. 둘째 줄에는 배열 A에 들어있는 정수가 주어진다. 배열에 들어있는 정수는 -100보다 크거나 같고, 100보다 작거나 같다.       

---

#### 출력

첫째 줄에 배열에 들어있는 수의 순서를 적절히 바꿔서 얻을 수 있는 식의 최댓값을 출력한다.

---
#### 예제 입력 1

~~~
6
20 1 15 8 4 10
~~~

#### 예제 출력 1

~~~
62
~~~

[출처](https://www.acmicpc.net/problem/10819)

---

### 문제풀이

이번 문제는 재귀가 필요한 완전 탐색 문제입니다.   

재귀를 사용하지 않으면 N의 값에 따라 for문의 개수를 다르게 해야 하므로 간결하게 짜기 어렵습니다.   

그래서 재귀를 사용해서 모든 경우의 수를 찾아서 가장 큰 결과를 반환하도록 했습니다.   

물론 파이썬에서 제공하는 itertools의 permutation을 사용하면 더 간단히 풀 수 있지만

재귀를 연습하는 측면에서 그냥 재귀를  풀었습니다.

---

#### 나의 풀이

~~~python
n = int(input())
a = list(map(int, input().split()))
visited = [False] * n

# 합을 구하는 함수
def get_sum(arr):
    s = 0
    for i in range(len(arr) - 1):
        s += abs(arr[i+1]-arr[i])
    return s
# 순열을 구하는 함수
def recursive(idx, arr, m):
    # 마지막에 도달하면 종료
    if idx == n:
        if get_sum(arr) > m:
            m = get_sum(arr)
        return m

    for i in range(n):
        if not visited[i]:
            visited[i] = True
            arr.append(a[i])
            m = recursive(idx + 1, arr, m)
            arr.pop()
            visited[i] = False

    return m

print(recursive(0, [], 0))
~~~

---

#### 다른 사람의 풀이

~~~python
n=int(input())

a=list(map(int,input().split()))
ans=0
from itertools import *
for i in permutations(a,n):
    ret=0
    l=list(i)
    for i in range(n-1):
        ret+=abs(l[i+1]-l[i])
    ans=max(ret,ans)
print(ans)
~~~
