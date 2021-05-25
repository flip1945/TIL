# 2개 이하로 다른 비트(Level 2)

### 문제 설명

2차원 행렬 arr1과 arr2를 입력받아, arr1에 arr2를 곱한 결과를 반환하는 함수, solution을 완성해주세요.    

---

#### 제한사항

* 행렬 arr1, arr2의 행과 열의 길이는 2 이상 100 이하입니다.

* 행렬 arr1, arr2의 원소는 -10 이상 20 이하인 자연수입니다.

* 곱할 수 있는 배열만 주어집니다.

---

#### 입출력 예

|arr1|	arr2|	return|
|-|-|-|
|\[\[1, 4], \[3, 2], \[4, 1]]|	\[\[3, 3], \[3, 3]]	|\[\[15, 15], \[15, 15], \[15, 15]]|
|\[\[2, 3, 2], \[4, 2, 4], \[3, 1, 4]]|	\[\[5, 4, 3], \[2, 4, 1], \[3, 1, 1]]|	\[\[22, 22, 11], \[36, 28, 18], \[29, 20, 14]]|

[출처](https://programmers.co.kr/learn/courses/30/lessons/12949)

---

### 문제풀이

이 문제는 행렬의 곱셈을 구현하는 문제입니다.    

그냥 주어진대로 구현하면 되서 글을 쓰려고 하지 않았는데,    

다른 분의 풀이가 너무 좋아서 글을 쓰게됐습니다.   

아래의 다른 사람의 풀이를 보면 zip(\*B)라는 부분이 있는데, 현재 리스트의 행과 열을 바꿔주는 문장입니다.   

zip함수를 이렇게 사용하는 것을 생각해 보지 못했는데, 너무 참신하고 좋았습니다.   

---

#### 나의 풀이

~~~python
def solution(arr1, arr2):
    answer = []

    for i in range(len(arr1)):
        answer.append([sum([arr1[i][k] * arr2[k][j] for k in range(len(arr2))]) for j in range(len(arr2[0]))])
        
    return answer
~~~

---

#### 다른 사람의 풀이

~~~python
def productMatrix(A, B):
    return [[sum(a*b for a, b in zip(A_row,B_col)) for B_col in zip(*B)] for A_row in A]
~~~
