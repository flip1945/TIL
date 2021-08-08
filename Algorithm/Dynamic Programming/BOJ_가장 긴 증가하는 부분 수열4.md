# 가장 긴 증가하는 부분 수열4 (Gold 4)

### 문제

수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.   

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {10, 20, 10, 30, 20, 50} 이고, 길이는 4이다.

---

#### 입력

첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000)이 주어진다.   

둘째 줄에는 수열 A를 이루고 있는 Ai가 주어진다. (1 ≤ Ai ≤ 1,000)

---

#### 출력

첫째 줄에 수열 A의 가장 긴 증가하는 부분 수열의 길이를 출력한다.   

둘째 줄에는 가장 긴 증가하는 부분 수열을 출력한다. 그러한 수열이 여러가지인 경우 아무거나 출력한다.

---

#### 예제 입력 1
~~~
6
10 20 10 30 20 50
~~~

#### 예제 출력 1
~~~
4
10 20 30 50
~~~

[출처](https://www.acmicpc.net/problem/14002)

---

### 문제풀이

이 문제는 [이전에 풀었던 문제](https://github.com/flip1945/TIL/blob/main/Algorithm/Dynamic%20Programming/BOJ_%EA%B0%80%EC%9E%A5%20%EA%B8%B4%20%EC%A6%9D%EA%B0%80%ED%95%98%EB%8A%94%20%EB%B6%80%EB%B6%84%20%EC%88%98%EC%97%B4.md)의 연장선입니다.

이번 문제에서는 최대 길이 뿐만 아니라 수열도 같이 출력해야 하는 부분이 추가됐습니다.   

이전 문제를 풀었던 방식과 동일한 방식으로 문제를 풀었고,

수열을 출력하기 위해 아래와 같은 방법을 사용했습니다.   

1. dp 테이블에서 역순으로 탐색하면서 수열을 선택합니다.
2. 가장 긴 수열을 찾고, 찾는다면 찾은 수열보다 길이가 1 작은 수열을 찾습니다.
3. 길이가 0이 될때까지 찾으면 모든 수열을 찾을 수 있습니다.
4. 정답 배열에 역순으로 저장됐기 때문에 다시 뒤집어서 출력해 줍니다.

---

#### 나의 풀이

~~~python
n = int(input())
nums = list(map(int, input().split()))
dp = [0] * n

for i in range(n):
    dp[i] = max([dp[j] for j in range(i) if nums[j] < nums[i]] + [0]) + 1

m = max(dp)
print(m)
# 역순으로 정답 수열을 찾음
answer = []
for i in range(n-1, -1, -1):
    if dp[i] == m:
        answer.append(nums[i])
        m -= 1

print(*answer[::-1])
~~~

---

#### 다른 사람의 풀이

~~~python
input()
a=list(map(int,input().split()))
b=[[n]for n in a]
for i in range(1,len(b)):
	for j in range(i):
		if b[j][-1]<b[i][-1]and len(b[j])>=len(b[i]):b[i]=b[j]+[a[i]]
c=max(b,key=len)
print(len(c))
print(' '.join(map(str,c)))
~~~
