# 쿼드압축 후 개수 세기(Level 2)

### 문제 설명

0과 1로 이루어진 2n x 2n 크기의 2차원 정수 배열 arr이 있습니다. 당신은 이 arr을 쿼드 트리와 같은 방식으로 압축하고자 합니다. 구체적인 방식은 다음과 같습니다.   

1. 당신이 압축하고자 하는 특정 영역을 S라고 정의합니다.   

2. 만약 S 내부에 있는 모든 수가 같은 값이라면, S를 해당 수 하나로 압축시킵니다.   

3. 그렇지 않다면, S를 정확히 4개의 균일한 정사각형 영역(입출력 예를 참고해주시기 바랍니다.)으로 쪼갠 뒤, 각 정사각형 영역에 대해 같은 방식의 압축을 시도합니다.   

arr이 매개변수로 주어집니다. 위와 같은 방식으로 arr을 압축했을 때, 배열에 최종적으로 남는 0의 개수와 1의 개수를 배열에 담아서 return 하도록 solution 함수를 완성해주세요.   

---

#### 제한사항

* arr의 행의 개수는 1 이상 1024 이하이며, 2의 거듭 제곱수 형태를 하고 있습니다. 즉, arr의 행의 개수는 1, 2, 4, 8, ..., 1024 중 하나입니다.
    * arr의 각 행의 길이는 arr의 행의 개수와 같습니다. 즉, arr은 정사각형 배열입니다.
    * arr의 각 행에 있는 모든 값은 0 또는 1 입니다.

---

#### 입출력 예

|arr|	result|
|-|-|
|\[\[1,1,0,0],\[1,0,0,0],\[1,0,0,1],\[1,1,1,1]]|	\[4,9]|
|\[\[1,1,1,1,1,1,1,1],\[0,1,1,1,1,1,1,1],\[0,0,0,0,1,1,1,1],\[0,1,0,0,1,1,1,1],\[0,0,0,0,0,0,1,1],\[0,0,0,0,0,0,0,1],\[0,0,0,0,1,0,0,1],\[0,0,0,0,1,1,1,1]]	|\[10,15]|

#### 입출력 예 설명

##### 입출력 예 #1

* 다음 그림은 주어진 arr을 압축하는 과정을 나타낸 것입니다.

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d6900862-8be4-4610-aaef-bc8efd5650cf/ex1.png" width = 700>

* 최종 압축 결과에 0이 4개, 1이 9개 있으므로, \[4,9]를 return 해야 합니다.

##### 입출력 예 #2

* 다음 그림은 주어진 arr을 압축하는 과정을 나타낸 것입니다.

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/952a05b7-5157-4211-82d9-02845c187e13/ex2.png" width = 700>

* 최종 압축 결과에 0이 10개, 1이 15개 있으므로, \[10,15]를 return 해야 합니다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/68936)

---

### 문제풀이

이 문제는 2차원 배열을 압축하는 문제입니다.   

2차원 배열을 4개로 나눠서 압축이 되는지 안 되는지 확인하면 됩니다.   

저는 2차원 배열을 변경하는 식으로 문제를 구현했는 데, 다른 사람의 풀이를 보니,  굳이 그렇게 할 필요는 없었습니다.   

실제로 리스트를 쪼개지 않고, 좌표로 참조하면 되기 때문입니다.   

다음에는 구현할 필요가 없는 부분은 하지 않고 문제를 풀어야 겠습니다.   

---

#### 나의 풀이

~~~python
def solution(arr):
    # 처음 리스트가 바로 압축 가능한 경우 예외 처리
    if sum([sum(q) for q in arr]) == 0 or sum([sum(q) for q in arr]) == len(arr)**2:
        zero = str(arr).count("0")
        return [1, 0] if zero else [0, 1]
    
    arr = divide(arr)
    
    zero = str(arr).count("0")
    one = str(arr).count("1")
    return [zero, one]

# 리스트를 4개로 쪼개고 압축하는 함수
def divide(arr):
    # 길이가 1인 경우는 더 이상 압축할 수 없는 경우
    if len(arr) == 1:
        return arr[0]
    # 리스트를 4개로 쪼갬
    quad = list(split_quad(arr))
    # 쪼갠 리스트를 전부 확인
    for i in range(len(quad)):
        # 타입이 int인 경우는 압축된 경우기 때문에 무시
        if type(quad[i]) == int:
            continue

        chk = 0
        # 압축이 가능한 지 확인
        for q in quad[i]:
            chk += sum(q)
        # 0으로 압축 가능한 경우 0으로 압축
        if not chk:
            quad[i] = [0]
        # 1로 압축 가능항 경우 1로 압축
        elif chk == len(quad[i])**2:
            quad[i] = [1]
    # 재귀 실행
    for i in range(len(quad)):
        quad[i] = divide(quad[i])
    # 결과 
    return quad

# 리스트를 4개로 쪼개는 함수
def split_quad(arr):
    half = len(arr)//2
    arr1 = [arr[i][:half] for i in range(half)]
    arr2 = [arr[i][half:] for i in range(half)]
    arr3 = [arr[i][:half] for i in range(half, half*2)]
    arr4 = [arr[i][half:] for i in range(half, half*2)]
    return [arr1] + [arr2] + [arr3] + [arr4]
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(arr):
    answer = [0, 0]

    def check(size, x, y):
        if size == 1:
            answer[arr[y][x]] += 1
            return
        else:
            first = arr[y][x]

            for dy in range(size):
                for dx in range(size):
                    if first != arr[y + dy][x + dx]:
                        check(size // 2, x, y)
                        check(size // 2, x + size // 2, y)
                        check(size // 2, x, y + size // 2)
                        check(size // 2, x + size // 2, y + size // 2)
                        return
            answer[first] += 1
    check(len(arr),0,0)


    return answer
~~~
