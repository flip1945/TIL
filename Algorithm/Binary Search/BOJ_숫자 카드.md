# 숫자 카드 (Silver 4)

### 문제

숫자 카드는 정수 하나가 적혀져 있는 카드이다. 상근이는 숫자 카드 N개를 가지고 있다. 정수 M개가 주어졌을 때, 이 수가 적혀있는 숫자 카드를 상근이가 가지고 있는지 아닌지를 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 상근이가 가지고 있는 숫자 카드의 개수 N(1 ≤ N ≤ 500,000)이 주어진다. 둘째 줄에는 숫자 카드에 적혀있는 정수가 주어진다. 숫자 카드에 적혀있는 수는 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다. 두 숫자 카드에 같은 수가 적혀있는 경우는 없다.   

셋째 줄에는 M(1 ≤ M ≤ 500,000)이 주어진다. 넷째 줄에는 상근이가 가지고 있는 숫자 카드인지 아닌지를 구해야 할 M개의 정수가 주어지며, 이 수는 공백으로 구분되어져 있다. 이 수도 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다.

---

#### 출력

첫째 줄에 입력으로 주어진 M개의 수에 대해서, 각 수가 적힌 숫자 카드를 상근이가 가지고 있으면 1을, 아니면 0을 공백으로 구분해 출력한다.

---

#### 예제 입력 1
~~~
5
6 3 2 10 -10
8
10 9 -5 2 3 4 5 -10
~~~

#### 예제 출력 1
~~~
1 0 0 1 1 0 0 1
~~~

[출처](https://www.acmicpc.net/problem/10815)

---

### 문제풀이

이번 문제는 이분 탐색 문제입니다.   

이분 탐색을 연습하기 위해서 푼 문제로 특정 값이 리스트 안에 존재하는지 확인하는 문제입니다.   

기본적인 이분 탐색 알고리즘을 사용하면 문제를 해결할 수 있습니다.

---

#### 나의 풀이

1. 정석정인 풀이

~~~python
input()
n = sorted(list(map(int, input().split())))
input()
m = list(map(int, input().split()))

for num in m:
    left, right = 0, len(n) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if num < n[mid]:
            right = mid - 1
        elif num > n[mid]:
            left = mid + 1
        else:
            break

    print(1 if num == n[mid] else 0, end=' ')
~~~

2. 약간 변형한 풀이

~~~python
input()
n = sorted(list(map(int, input().split())))
input()
m = list(map(int, input().split()))

for num in m:
    left, right = 0, len(n) - 1
    while left < right:
        mid = left + (right - left) // 2
        if num <= n[mid]:
            right = mid
        else:
            left = mid + 1
    print(1 if num == n[right] else 0, end=' ')
~~~

---

#### 다른 사람의 풀이

~~~python
input()
s = set(input().split())
input()
print(" ".join("1" if n in s else "0" for n in input().split()))
~~~
