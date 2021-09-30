# 스킬트리 (Level 2)

### 문제 설명

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.   

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더`일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.   

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 `스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크`나 `라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.   

선행 스킬 순서 skill과 유저들이 만든 스킬트리(유저가 스킬을 배울 순서)를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.   

---

#### 제한 조건

* 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.

* 스킬 순서와 스킬트리는 문자열로 표기합니다.
    * 예를 들어, C → B → D 라면 "CBD"로 표기합니다

* 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.

* skill_trees는 길이 1 이상 20 이하인 배열입니다.

* skill_trees의 원소는 스킬을 나타내는 문자열입니다.
    * skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

---

#### 입출력 예

|skill|	skill_trees|	return|
|-|-|-|
|"CBD"|\["BACDE", "CBADF", "AECB", "BDA"]|	2|

#### 입출력 예 설명

* "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.

* "CBADF": 가능한 스킬트리입니다.

* "AECB": 가능한 스킬트리입니다.

* "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.  

출처 : https://programmers.co.kr/learn/courses/30/lessons/49993

---

### 문제풀이

이번 문제는 완전탐색 문제입니다.   

스킬을 정해진 순서대로 배워야 하는 문제입니다.   

"ABC"라는 스킬 순서가 있다면 "A", "AB", "ABC"는 가능하고 "BA, "CB", "BC"등은 불가능 합니다.    

이를 확인하기 위해서 skill_trees의 모든 문자를 일일이 확인해야 합니다.   

선행 스킬 순서와 현재 배우는 스킬을 비교할 때, for-else 문을 이용해서 정답을 제출했습니다.   

---

#### 나의 풀이

~~~python
def solution(skill, skill_trees):
    answer = 0
    # 모든 skill_tree를 확인
    for skill_tree in skill_trees:
        skill_cnt = 0
        # skill_tree의 모든 문자를 확인
        for s in skill_tree:
            # 현재 문자열이 선행 스킬이라면
            if s in skill:
                # 현재 문자열 스킬과 배워야 하는 선행 스킬의 순서가 다르다면 종료
                if skill[skill_cnt] != s:
                    break
                # 같다면 확인해야 하는 선행 스킬의 순서를 증가시킴
                skill_cnt += 1
        # 선행 스킬 순서대로 배웠다면 정답을 1증가시킴
        else:
            answer += 1
    return answer
~~~

#### 다른 사람의 풀이

~~~python
def solution(skill, skill_trees):
    answer = 0

    for skills in skill_trees:
        skill_list = list(skill)

        for s in skills:
            if s in skill:
                if s != skill_list.pop(0):
                    break
        else:
            answer += 1

    return answer
~~~
