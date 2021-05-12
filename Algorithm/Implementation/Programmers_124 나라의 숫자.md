# 124 나라의 숫자(Level 2)

### 문제 설명

124 나라가 있습니다. 124 나라에서는 10진법이 아닌 다음과 같은 자신들만의 규칙으로 수를 표현합니다.

1. 124 나라에는 자연수만 존재합니다.
2. 124 나라에는 모든 수를 표현할 때 1, 2, 4만 사용합니다.

예를 들어서 124 나라에서 사용하는 숫자는 다음과 같이 변환됩니다.

|10진법|	124 나라|	10진법|	124 나라|
|-|-|-|-|
|1|	1|	6|	14|
|2|	2|	7|	21|
|3|	4|	8|	22|
|4|	11|	9|	24|
|5|	12|	10|	41|

자연수 n이 매개변수로 주어질 때, n을 124 나라에서 사용하는 숫자로 바꾼 값을 return 하도록 solution 함수를 완성해 주세요.

---

#### 제한사항

* n은 500,000,000이하의 자연수 입니다.

---

#### 입출력 예

|n|	result|
|-|-|
|1|	1|
|2|	2|
|3|	4|
|4|	11|

[출처](https://programmers.co.kr/learn/courses/30/lessons/12899)

---

### 문제풀이

이 문제는 비교적 쉬운 구현 문제입니다.   

그런데 저는 이 문제의 풀이를 떠올릴 수 없어서 너무 복잡한 방법으로 풀게 됐습니다.   

자리수가 변하는 숫자를 리스트로 만들고 각 자리수 마다 무슨 숫자가 나올지 처리까지 해줬습니다.   

너무 어렵게 풀어서 다른 풀이방법이 있을 것이라고 생각하긴 했는데, 역시 쉬운 풀이법이 있었습니다.   

숫자 n에 1을 빼고, n을 3으로 나눕니다.   
그리고 n을 몫으로 바꾸고, 나눈 나머지를 가지고 1,2,4를 표시하면 됩니다.   

이것을 n이 0이 되기 전까지 반복하면 되는 간단한 문제였습니다.   

어렵고 이상한 방법으로 풀긴 했지만 다른 사람의 정답을 보지 않고 푼 것이 의미 있었다고 생각합니다.   

---

#### 나의 풀이

~~~python
def solution(n):
    answer = ''
    nums = {1:"1", 2:"2", 3:"4"}
    decade = [0]
    i = 1
    
    while decade[-1] < n:
        decade.append(decade[i-1] + (3**i))
        i += 1
        
    head = -2
    for j in range(len(decade) - 2, 0, -1):
        num = 1
        for k in [1, 2]:
            if n - decade[head] > k * 3**j:
                num = k + 1
        answer += nums[num]
        n -= num * 3**j
        head -= 1
    # 한자리 예외처리
    if not n:
        n = 3
    answer += nums[n]
        
    return answer
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(n):
    num = ['1','2','4']
    answer = ""

    while n > 0:
        n -= 1
        answer += num[n % 3]
        n //= 3
    
    return "".join(reversed(list(answer)))
~~~

~~~python
def change124(n):
    if n<=3:
        return '124'[n-1]
    else:
        q, r = divmod(n-1, 3) 
        return change124(q) + '124'[r]
~~~

~~~python
def change124(n):
    answer = ''
    while n:
        if n%3 == 1:
            answer+='1'
            n = n // 3
        elif n%3 == 2:
            answer+='2'
            n = n // 3
        elif n%3 == 0:
            answer+='4'
            n = n // 3 - 1

    list1 = list(answer)
    list1.reverse()
    return ''.join(list1)
~~~
