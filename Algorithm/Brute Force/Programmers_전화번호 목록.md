# 메뉴 리뉴얼 (Level 2)

### 문제 설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.   

전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.   

* 구조대 : 119

* 박준영 : 97 674 223

* 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.   

---

#### 제한사항

* phone_book의 길이는 1 이상 1,000,000 이하입니다.

    * 각 전화번호의 길이는 1 이상 20 이하입니다.

    * 같은 전화번호가 중복해서 들어있지 않습니다.

---

#### 입출력 예

|phone_book|	return|
|-|-|
|\["119", "97674223", "1195524421"]|	false|
|\["123","456","789"]|	true|
|\["12","123","1235","567","88"]|	false|

#### 입출력 예에 대한 설명

##### 입출력 예 #1

앞에서 설명한 예와 같습니다.

##### 입출력 예 #2

한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

##### 입출력 예 #3

첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다. 

출처 : https://programmers.co.kr/learn/courses/30/lessons/42577

---

### 문제풀이

이번 문제는 Hash를 사용해서 완전탐색을 하는 문제입니다.   

모든 전화번호를 확인하고, 확인한 전화번호와 다른 전화번호의 앞자리와 같은지 탐색하는 문제입니다.   

for문으로 모든 전화번호를 확인하는 데, O(n)의 시간이 걸리고, 확인한 전화번호가 다른 전화번호의 접두어인지 확인하는데, O(n)의 시간이 걸립니다.   

평범하게 리스트를 사용하면 O(n^2)의 시간이 걸려서 문제를 통과할 수 없게됩니다.   

하지만 dictionary(Hash)를 사용하면 O(1) 시간에 현재 확인하고 있는 전화번호가 다른 전화번호의 접두어인지 알 수 있습니다.   

이 문제를 풀면서 든 생각은 
Hash Type을 Value를 사용하지 않고, Hash함수 사용을 위해 Key만 사용하는 것이 참신하다고 생각했습니다.

---

#### 나의 풀이

1. 정답으로 제출한 풀이

~~~python
def solution(phone_book):
    len_list = sorted(list(set([len(p) for p in phone_book])))
    dic = {key:0 for key in phone_book}

    for phone in phone_book:
        for l in len_list:
            if l <= len(phone) and phone[:l] in dic:
                dic[phone[:l]] += 1
    return False if max(dic.values()) > 1 else True
~~~

2. 제출 후 다듬은 풀이

~~~python
def solution(phone_book):
    len_list = list(set([len(p) for p in phone_book]))
    dic = {key : 1 for key in phone_book}
    
    for phone in phone_book:
        for length in len_list:
            if len(phone) > length and phone[:length] in dic:
                return False
    return True
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(phone_book):
    answer = True
    hash_map = {}
    for phone_number in phone_book:
        hash_map[phone_number] = 1
    for phone_number in phone_book:
        temp = ""
        for number in phone_number:
            temp += number
            if temp in hash_map and temp != phone_number:
                answer = False
    return answer
~~~
