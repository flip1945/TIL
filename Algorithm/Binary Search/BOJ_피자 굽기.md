# 피자 굽기 (Gold 5)

### 문제

월드피자 원주 지점에서 N개의 피자 반죽을 오븐에 넣고 구우려고 한다. 그런데, 월드피자에서 만드는 피자 반죽은 지름이 제각각이다. 그런가하면, 월드피자에서 사용하는 오븐의 모양도 몹시 오묘하다. 이 오븐은 깊은 관처럼 생겼는데, 관의 지름이 깊이에 따라 들쭉날쭉하게 변한다. 아래는 오븐의 단면 예시이다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201006/pizz1.PNG">

피자 반죽은 완성되는 순서대로 오븐에 들어간다. 이렇게 N개의 피자가 오븐에 모두 들어가고 나면, 맨 위의 피자가 얼마나 깊이 들어가 있는지가 궁금하다. 이를 알아내는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 오븐의 깊이 D와 피자 반죽의 개수 N이 공백을 사이에 두고 주어진다. (1 ≤ D, N ≤ 300,000) 둘째 줄에는 오븐의 최상단부터 시작하여 깊이에 따른 오븐의 지름이 차례대로 주어진다. 셋째 줄에는 피자 반죽이 완성되는 순서대로, 그 각각의 지름이 주어진다. 오븐의 지름이나 피자 반죽의 지름은 10억 이하의 자연수이다.

---

#### 출력

첫째 줄에, 마지막 피자 반죽의 위치를 출력한다. 오븐의 최상단이 1이고, 최하단 가장 깊은 곳이 D이 된다. 만약 피자가 모두 오븐에 들어가지 않는다면, 0을 출력한다.

---

#### 예제 입력 1
~~~
7 3
5 6 4 3 6 2 3
3 2 5
~~~

#### 예제 출력 1
~~~
2
~~~

---

#### 힌트

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201006/piz2.PNG">

출처 : https://www.acmicpc.net/problem/1756

---

### 문제풀이

이번 문제는 사실 구현 문제입니다.   

그런데 저는 이분 탐색 연습을 위해서 이분 탐색으로 문제를 풀었습니다.   

피자가 들어갈 오븐의 위치를 이분 탐색을 통해 찾고, 다음에 찾는 위치의 한계를 1칸 씩 올렸습니다.   

이런 방법으로 모든 피자가 들어갈 위치를 찾으면 문제를 해결할 수 있습니다.   

더 쉬운 방법으로 푼 풀이는 아래 다른 사람의 풀이에 첨부했습니다.

---

#### 나의 풀이

~~~python
d, n = map(int, input().split())
oven = list(map(int, input().split()))
pizza = list(map(int, input().split()))

for i in range(1, d):
    cut = oven[i-1]
    if oven[i] > cut:
        oven[i] = cut

limit = d

for p in pizza:
    left, right = 0, limit - 1

    while left <= right:
        mid = left + (right - left) // 2

        if oven[mid] >= p:
            left = mid + 1
        else:
            right = mid - 1
    limit = right

print(limit + 1 if limit >= 0 else 0)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/11795969

~~~python
input()

oven = [*map(int,input().split())]
pizza = [*map(int,input().split())]

m = [1012345678]
for o in oven:

	m.append(min(m[-1], o))

i = len(m) - 1

for p in pizza:
	if i == 0:
		ans = 0
		break
	while i >= 0 and m[i] < p:
		i-=1

	ans = i
	i-=1

print(ans)
~~~
