# 위장(Level 2)

### 문제 설명

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.   

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.   

|종류|	이름|
|-|-|
|얼굴|	동그란 안경, 검정 선글라스|
|상의|	파란색 티셔츠|
|하의|	청바지|
|겉옷|	긴 코트|

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

---

#### 제한사항

* clothes의 각 행은 \[의상의 이름, 의상의 종류]로 이루어져 있습니다.

* 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.

* 같은 이름을 가진 의상은 존재하지 않습니다.

* clothes의 모든 원소는 문자열로 이루어져 있습니다.

* 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '\_' 로만 이루어져 있습니다.

* 스파이는 하루에 최소 한 개의 의상은 입습니다.

---

#### 입출력 예

|clothes|	return|
|-|-|
|\[\["yellowhat", "headgear"],\["bluesunglasses", "eyewear"], \["green_turban", "headgear"]]|	5|
|\[\["crowmask", "face"], \["bluesunglasses", "face"], \["smoky_makeup", "face"]]|	3|

#### 입출력 예 설명

##### 예제 #1

headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.
~~~
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
~~~

##### 예제 #2

face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.
~~~
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
~~~

[출처](https://programmers.co.kr/learn/courses/30/lessons/42578)

---

### 문제풀이

이 문제는 Hash로 옷 종류별로 개수를 저장하는 문제입니다.   

옷의 종류별 개수만 알고 있으면 수식으로 정답을 알 수 있습니다.   

처음에는 조합을 사용해서 풀었는데, 1번 케이스가 시간 초과해서 문제에 대해 더 알아봤습니다.   

검색 결과 의상 종류별로 + 1 한 값을 전부 곱하고, 결과에 - 1을 합니다.   

글로만 보면 이해하기 어려운데 아래에서 예시로 설명하겠습니다.   

예시)   

모자 : 3개, 상의 : 4개, 하의 7개   

이 경우 (모자의 수 + 1) * (상의의 수 + 1) * (하의의 수 + 1) - 1이 정답입니다.
(4 * 5 * 8 - 1 = 159)   

위 식은 경우의 수를 계산하는 것인데, 각 의상 종류에서 +1을 하는 이유는 모자, 상의, 하의를 입지 않은 경우도 계산하기 위해서입니다.   

그리고 마지막에 -1을 하는 이유는 아무것도 입지 않은 경우를 제외해야 하기 때문입니다.   

최종 결과 : (모자 3개 + 안 입은 경우 1개) * (상의 4개 + 안 입은 경우 1개) * (하의 7개 + 안 입은 경우 1개) - 아무것도 입지 않은 경우 1개   

---

#### 나의 풀이

~~~python
def solution(clothes):
    answer = 1
    clothes_type = {c[1] : 0 for c in clothes}
    
    for c in clothes:
        clothes_type[c[1]] += 1
        
    nums = list(clothes_type.values())
    # 각 의상에 +1한 값을 모두 곱함
    for num in nums:
        answer *= num + 1
    # 아무것도 입지 않은 경우를 제외
    return answer - 1
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(clothes):
    from collections import Counter
    from functools import reduce
    cnt = Counter([kind for name, kind in clothes])
    answer = reduce(lambda x, y: x*(y+1), cnt.values(), 1) - 1
    return answer
~~~
