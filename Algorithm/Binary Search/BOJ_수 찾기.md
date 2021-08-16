# 수 찾기 (Silver 4)

### 문제

N개의 정수 A\[1], A\[2], …, A\[N]이 주어져 있을 때, 이 안에 X라는 정수가 존재하는지 알아내는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 자연수 N(1 ≤ N ≤ 100,000)이 주어진다. 다음 줄에는 N개의 정수 A\[1], A\[2], …, A\[N]이 주어진다. 다음 줄에는 M(1 ≤ M ≤ 100,000)이 주어진다. 다음 줄에는 M개의 수들이 주어지는데, 이 수들이 A안에 존재하는지 알아내면 된다. 모든 정수의 범위는 -2^31 보다 크거나 같고 2^31보다 작다.

---

#### 출력

M개의 줄에 답을 출력한다. 존재하면 1을, 존재하지 않으면 0을 출력한다.

---

#### 예제 입력 1
~~~
5
4 1 5 2 3
5
1 3 7 9 5
~~~

#### 예제 출력 1
~~~
1
1
0
0
1
~~~

[출처](https://www.acmicpc.net/problem/1920)

---

### 문제풀이

이번 문제는 이분 탐색으로 분류돼있는 문제입니다.   

저는 이 문제를 보고 이분 탐색으로 풀어야 겠다고 생각했고, 이분 탐색으로 문제를 풀었습니다.   

하지만 문제를 다 풀고 다른 사람의 풀이를 보니 HashTable을 이용해 푸는 방법도 있다는 것을 알게됐습니다.   

---

#### 나의 풀이

~~~python
n = int(input())
a = sorted(list(map(int, input().split())))
m = int(input())
nums = list(map(int, input().split()))
nums.reverse()

while nums:
    num = nums.pop()

    low = 0
    high = n

    while low < high:
        mid = (low + high) // 2

        if num >= a[mid]:
            low = mid + 1
        else:
            high = mid

    print(1 if a[high-1] == num else 0)
~~~

---

#### 다른 사람의 풀이

~~~python
n = input()
a = set(input().split())
m = input()
b = input().split()

for i in b:
    print(1 if i in a else 0)
~~~
