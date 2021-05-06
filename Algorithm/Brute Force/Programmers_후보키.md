# 후보키(Level 2)

### 문제 설명

프렌즈대학교 컴퓨터공학과 조교인 제이지는 네오 학과장님의 지시로, 학생들의 인적사항을 정리하는 업무를 담당하게 되었다.   

그의 학부 시절 프로그래밍 경험을 되살려, 모든 인적사항을 데이터베이스에 넣기로 하였고, 이를 위해 정리를 하던 중에 후보키(Candidate Key)에 대한 고민이 필요하게 되었다.   

후보키에 대한 내용이 잘 기억나지 않던 제이지는, 정확한 내용을 파악하기 위해 데이터베이스 관련 서적을 확인하여 아래와 같은 내용을 확인하였다.   

* 관계 데이터베이스에서 릴레이션(Relation)의 튜플(Tuple)을 유일하게 식별할 수 있는 속성(Attribute) 또는 속성의 집합 중, 다음 두 성질을 만족하는 것을 후보 키(Candidate Key)라고 한다.
    * 유일성(uniqueness) : 릴레이션에 있는 모든 튜플에 대해 유일하게 식별되어야 한다.
    * 최소성(minimality) : 유일성을 가진 키를 구성하는 속성(Attribute) 중 하나라도 제외하는 경우 유일성이 깨지는 것을 의미한다. 즉, 릴레이션의 모든 튜플을 유일하게 식별하는 데 꼭 필요한 속성들로만 구성되어야 한다.

제이지를 위해, 아래와 같은 학생들의 인적사항이 주어졌을 때, 후보 키의 최대 개수를 구하라.

<img src = "https://grepp-programmers.s3.amazonaws.com/files/production/f1a3a40ede/005eb91e-58e5-4109-9567-deb5e94462e3.jpg">

위의 예를 설명하면, 학생의 인적사항 릴레이션에서 모든 학생은 각자 유일한 "학번"을 가지고 있다. 따라서 "학번"은 릴레이션의 후보 키가 될 수 있다.   

그다음 "이름"에 대해서는 같은 이름("apeach")을 사용하는 학생이 있기 때문에, "이름"은 후보 키가 될 수 없다. 그러나, 만약 ["이름", "전공"]을 함께 사용한다면 릴레이션의 모든 튜플을 유일하게 식별 가능하므로 후보 키가 될 수 있게 된다.   

물론 ["이름", "전공", "학년"]을 함께 사용해도 릴레이션의 모든 튜플을 유일하게 식별할 수 있지만, 최소성을 만족하지 못하기 때문에 후보 키가 될 수 없다.   

따라서, 위의 학생 인적사항의 후보키는 "학번", ["이름", "전공"] 두 개가 된다.   

릴레이션을 나타내는 문자열 배열 relation이 매개변수로 주어질 때, 이 릴레이션에서 후보 키의 개수를 return 하도록 solution 함수를 완성하라.   

---

#### [제한사항]

* relation은 2차원 문자열 배열이다.
* relation의 컬럼(column)의 길이는 1 이상 8 이하이며, 각각의 컬럼은 릴레이션의 속성을 나타낸다.
* relation의 로우(row)의 길이는 1 이상 20 이하이며, 각각의 로우는 릴레이션의 튜플을 나타낸다.
* relation의 모든 문자열의 길이는 1 이상 8 이하이며, 알파벳 소문자와 숫자로만 이루어져 있다.
* relation의 모든 튜플은 유일하게 식별 가능하다.(즉, 중복되는 튜플은 없다.)

---

#### 입출력 예

|relation|	result|
|-|-|
|[["100","ryan","music","2"],["200","apeach","math","2"],["300","tube","computer","3"],["400","con","computer","4"],["500","muzi","music","3"],["600","apeach","music","2"]]|	2|

#### 입출력 예 설명

##### 입출력 예 #1

문제에 주어진 릴레이션과 같으며, 후보 키는 2개이다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/42890)

---

### 문제풀이

이 문제는 모든 키 조합을 확인하는 완전탐색 문제입니다.   

문제를 풀 로직을 짜는 데, 시간이 많이 걸린 문제였습니다.   

심지어 로직을 잘 못 짜는 바람에 다시 처음부터 해서 더 그랬습니다.   

로직을 잘 못 짠 이유는 유일성을 만족하는 키의 조합을 만드는데 어려움을 느껴서입니다.  

모든 조합을 확인해야 하는데, 그렇게 하지 못해서 다시 짜게 됐습니다.   

문제를 해결한 방법은 문제 설명에서 주어진 정보를 가지고 그대로 구현했던 것입니다.   

> super key를 만들 수 있는 모든 조합을 만들어서 유일성을 확인하고, 그 조합에서 최소성을 만족하는 key들만 후보키로 지정했습니다. 

---

#### 나의 풀이

~~~python
from itertools import combinations as cb

def solution(relation):
    row = len(relation)
    col = len(relation[0])
    combi = []
    super_key = []
    candidate_key = []
    
    # 가능한 모든 조합을 combi에 저장
    for i in range(1, col + 1):
        combi += cb(range(col), i)
    # 모든 조합을 검사
    for com in combi:
        # 튜플들을 임시로 저장할 리스트
        tmp_tuples = []
        # 조합별로 모든 행을 확인
        for r in range(row):
            # 조합 정보를 담을 임시 리스트
            tmp = []
            for c in com:
                tmp.append(relation[r][c])
            tmp_tuples.append(tuple(tmp))
        # 현재 조합이 유일성을 만족하면 현재 조합을 super_key 리스트에 저장
        if len(set(tmp_tuples)) == row:
            super_key.append(com)
            
    # super_key 중 최소성을 만족하는 candidate_key를 탐색
    while True:
        # 새로운 super_key 정보가 담길 리스트
        new_super_key = []
        # super_key 중 가장 앞에 있는 key를 cadidate_key로 지정
        candidate_key.append(super_key[0])
        
        for s in super_key:
            # candidate_key가 포함된 super_key는 최소성을 만족하지 못하기 때문에 그 super_key를 제외하고, 새로운 super_key 리스트를 생성
            if not (set(super_key[0]) & set(s) == set(super_key[0])):
                new_super_key.append(s)
        # super_key를 새로운 new_super_key로 갱신
        super_key = new_super_key
        # 더 이상 확인할 수 있는 key가 존재하지 않는다면 종료
        if not new_super_key:
            break
        
    return len(candidate_key)
~~~
