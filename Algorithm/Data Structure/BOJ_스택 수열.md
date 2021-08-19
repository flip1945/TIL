# 스택 수열 (Silver 3)

### 문제

스택 (stack)은 기본적인 자료구조 중 하나로, 컴퓨터 프로그램을 작성할 때 자주 이용되는 개념이다. 스택은 자료를 넣는 (push) 입구와 자료를 뽑는 (pop) 입구가 같아 제일 나중에 들어간 자료가 제일 먼저 나오는 (LIFO, Last in First out) 특성을 가지고 있다.   

1부터 n까지의 수를 스택에 넣었다가 뽑아 늘어놓음으로써, 하나의 수열을 만들 수 있다. 이때, 스택에 push하는 순서는 반드시 오름차순을 지키도록 한다고 하자. 임의의 수열이 주어졌을 때 스택을 이용해 그 수열을 만들 수 있는지 없는지, 있다면 어떤 순서로 push와 pop 연산을 수행해야 하는지를 알아낼 수 있다. 이를 계산하는 프로그램을 작성하라.

---

#### 입력

첫 줄에 n (1 ≤ n ≤ 100,000)이 주어진다. 둘째 줄부터 n개의 줄에는 수열을 이루는 1이상 n이하의 정수가 하나씩 순서대로 주어진다. 물론 같은 정수가 두 번 나오는 일은 없다.

---

#### 출력

입력된 수열을 만들기 위해 필요한 연산을 한 줄에 한 개씩 출력한다. push연산은 +로, pop 연산은 -로 표현하도록 한다. 불가능한 경우 NO를 출력한다.

---

#### 예제 입력 1
~~~
8
4
3
6
8
7
5
2
1
~~~

#### 예제 출력 1
~~~
+
+
+
+
-
-
+
+
-
+
+
-
-
-
-
-
~~~

#### 예제 입력 2
~~~
5
1
2
5
3
4
~~~

#### 예제 출력 2
~~~
NO
~~~

---

#### 힌트

1부터 n까지에 수에 대해 차례로 \[push, push, push, push, pop, pop, push, push, pop, push, push, pop, pop, pop, pop, pop] 연산을 수행하면 수열 \[4, 3, 6, 8, 7, 5, 2, 1]을 얻을 수 있다.

[출처](https://www.acmicpc.net/problem/1874)

---

### 문제풀이

이번 문제는 stack을 이용해 푸는 문제입니다.   

처음에 문제를 읽었을 때, 풀이가 잘 안 떠올랐지만 5분 정도 생각하고 풀이를 떠올릴 수 있었습니다.   

기본적으로 이 문제는 stack을 이용한 구현을 통해 같은 결과가 나올 수 있는지 확인하는 문제입니다.   

문제의 의도대로 구현을 해서 풀었는데, 깔끔하게 풀지 못 한 것 같아서 아쉽습니다.   

다른 사람의 풀이를 보니 좀 더 깔끔하게 푼 것을 볼 수 있습니다.   

---

#### 나의 풀이

~~~python
input = __import__("sys").stdin.readline
n = int(input())
nums = [int(input()) for i in range(n)]

cnt = 0
i = 0
j = 1
stack = []
answer = []
# n * 2는 push, pop을 모두 한 결과의 숫자
while cnt < n * 2:
    if stack and stack[-1] == nums[i]:
        answer.append("-")
        stack.pop()
        i += 1
    else:
        answer.append("+")
        stack.append(j)
        j += 1
    cnt += 1
# 확인한 숫자가 1~n의 범위 밖의 숫자라면 만들 수 없는 수열
if j > n + 1:
    print("NO")
else:
    print(*answer, sep='\n')
~~~

---

#### 다른 사람의 풀이

~~~python
n=int(input())
s=[]
r=[]
c=1
for i in range(1,n+1):
    d=int(input())
    while c<=d:
        s.append(c)
        r.append("+")
        c+=1
    if s[-1]==d:
        r.append("-")
        s.pop()
    else:
        print("NO")
        break
else:
    print("\n".join(r))
~~~
