# 짝지어 제거하기(Level 2)

### 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.   

`섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)`   

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.   

Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.    

---

#### 제한사항

* scoville의 길이는 2 이상 1,000,000 이하입니다.

* K는 0 이상 1,000,000,000 이하입니다.

* scoville의 원소는 각각 0 이상 1,000,000 이하입니다.

* 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

---

#### 입출력 예

|scoville|	K|	return|
|-|-|-|
|[1, 2, 3, 9, 10, 12]|	7|	2|

#### 입출력 예 설명

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.   
   새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5   
   가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.   
   새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13   
   가진 음식의 스코빌 지수 = [13, 9, 10, 12]   

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.   

[출처](https://programmers.co.kr/learn/courses/30/lessons/42626)

---

### 문제풀이

이 문제는 heap의 기초 문제입니다.   

python에서 기본 제공되는 heap 모듈을 쓰면 heap을 간단하게 사용할 수 있습니다.   

heap을 써서 문제를 풀었는데, que를 이용하면 푸는 시간이 절반으로 줄어든다는 글이 있어서 que로 구현해서도 풀어봤습니다.   

que로 구현할 때 핵심은 '새로 섞은 음식은 항상 앞에 섞은 음식보다 값이 크다.'는 점입니다.   

이를 이용하면 새로운 음식 큐의 front는 섞은 음식 중 가장 작은 값으로 유지할 수 있습니다.   

일부러 다른 방식으로 풀어봤는데, 여러 관점으로 생각해 볼 수 있는 기회가 돼서 좋았습니다.

---

#### 나의 풀이

1. heap을 이용한 풀이

~~~python
import heapq

def solution(scoville, K):
    answer = 0
    # 스코빌 리스트를 최소 힙으로 변환함
    heapq.heapify(scoville)
    # 스코빌 힙의 가장 작은 값이 K보다 크면 종료
    while scoville[0] < K:
        # 스코빌 힙의 크기가 2보다 작다면 음식을 섞을 수 없으므로 -1을 반환
        if len(scoville) < 2:
            return -1
        # 가장 작은 값을 h1에 저장
        h1 = heapq.heappop(scoville)
        # 두번째로 작은 값을 h2에 저장
        h2 = heapq.heappop(scoville)
        # h1, h2를 섞은 새로운 음식을 스코빌 힙에 추가
        heapq.heappush(scoville, h1 + h2*2)
        # 반복횟수를 1씩 증가
        answer += 1
    
    return answer
~~~

2. 큐를 이용한 풀이

~~~python
from collections import deque

def solution(scoville, K):
    answer = 0
    
    s_que = deque(sorted(scoville))
    m_que = deque()
    
    while True:
        if s_que and m_que:
            if s_que[0] > K and m_que[0] > K:
                return answer
        elif m_que:
            if m_que[0] > K:
                return answer
        elif s_que:
            if s_que[0] > K:
                return answer
            
        if len(s_que) + len(m_que) < 2:
            return -1
        
        min_list = []
        
        for i in range(2):
            if s_que and m_que:
                if s_que[0] < m_que[0]:
                    min_list.append(s_que.popleft())
                else:
                    min_list.append(m_que.popleft())
            elif s_que:
                min_list.append(s_que.popleft())
            elif m_que:
                min_list.append(m_que.popleft())

        m_que.append(min_list[0] + min_list[1] * 2)
        answer += 1
        
    return answer
~~~
