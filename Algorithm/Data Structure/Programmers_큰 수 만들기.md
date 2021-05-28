# 큰 수 만들기(Level 2)

### 문제 설명

어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.   

예를 들어, 숫자 1924에서 수 두 개를 제거하면 \[19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.   

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.   

---

#### 제한사항

* number는 1자리 이상, 1,000,000자리 이하인 숫자입니다.

* k는 1 이상 `number의 자릿수` 미만인 자연수입니다.

---

#### 입출력 예

|number|	k|	return|
|-|-|-|
|"1924"|	2|	"94"|
|"1231234"|	3|	"3234"|
|"4177252841"|	4|	"775841"|

[출처](https://programmers.co.kr/learn/courses/30/lessons/42883)

---

### 문제풀이

이 문제는 Programmers에서 탐욕법으로 분류돼있는 문제입니다.   

저는 이 문제를 예전에 도전했었는데, 그 때 테스트 케이스 1개가 시간초과를 해서 통과하지 못했습니다.   

그 때 짠 로직은 시간복잡도가 O(n^2)이었습니다.    

이번에 풀 때는 시간복잡도를 줄여야 겠다고 마음 먹고 풀었는데, 도저히 탐욕법을 사용하는 방법으로는 로직이 떠오르지 않았습니다.   

그래서 질문하기에서 힌트를 좀 찾아봤는데, 이 문제를 stack으로 푸는 방법이 있다는 것을 알게 됐습니다.   

stack을 사용하니 이전과 같은 방식을 사용했음에도 바로 통과했습니다.   

---

#### 나의 풀이

~~~python
def solution(number, k):
    answer = ''
    end = 0
    stack = []
    
    for i, num in enumerate(number):
        # stack이 있고, stack의 top이 현재 숫자 보다 작다면
        if stack and stack[-1] < num:
            # 현재 숫자보다 같거나 큰 숫자가 나올 때까지 stack을 pop
            while stack:
                if stack[-1] >= num:
                    break
                stack.pop()
                end += 1
                # end 조건을 만족하면 결과를 반환
                if end == k:
                    return "".join(stack + [number[i:]])
        # 현재 숫자를 stack에 push
        stack.append(num)
    # 종료 조건을 만족하지 않은 경우 stack을 뒷부분을 짤라서 결과를 
    return "".join(stack[:-(k-end)])
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(number, k):
    stack = [number[0]]
    for num in number[1:]:
        while len(stack) > 0 and stack[-1] < num and k > 0:
            k -= 1
            stack.pop()
        stack.append(num)
    if k != 0:
        stack = stack[:-k]
    return ''.join(stack)
~~~
