# 타겟넘버(Level 2)

### 문제 설명

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

-1+1+1+1+1 = 3   
+1-1+1+1+1 = 3   
+1+1-1+1+1 = 3   
+1+1+1-1+1 = 3   
+1+1+1+1-1 = 3   

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

---

#### 제한사항

주어지는 숫자의 개수는 2개 이상 20개 이하입니다.   
각 숫자는 1 이상 50 이하인 자연수입니다.   
타겟 넘버는 1 이상 1000 이하인 자연수입니다.   

| numbers         | target | return |
| --------------- | ------ | ------ |
| [1, 1, 1, 1, 1] | 3      | 5      |

---

#### 입출력 예 설명
문제에 나온 예와 같습니다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/43165)

---

### 문제풀이

숫자리스트가 있고, 이 숫자들을 더하거나 빼서 원하는 숫자를 만들 수 있는 개수를 찾는 문제입니다.   

처음 보면 완전탐색을 해야 하는 걸로 보이는데, for문을 이용하면 일반적인 방법으로는 해결할 수 없습니다.   

왜냐하면 numbers의 개수가 달라질 때, for문의 개수를 수정해야 하기 때문에 그렇습니다.

그래서 저는 재귀탐색(dfs)을 이용해서 풀었습니다.   

~~~python
def solution(numbers, target):
    answer = dfs(numbers, 0, 0, [])
    # target 이 얼마나 등장했는지 결과를 반환
    return answer.count(target)


# node : 숫자 리스트 / cur_node : 현재 위치한 노드
# result : 결과값 / result_list : 결과값을 반환하기 위한 Parameter
def dfs(node, cur_node, result, result_list):
    # 기저 조건 : 현재 확인하는 노드가 마지막 노드라면 종료
    if cur_node == len(node):
        # 마지막 노드에서의 결과값을 결과값 리스트에 저장
        result_list.append(result)
        return
    # 다음 노드에 +, - 연산을 한 결과를 dfs 로 탐색
    dfs(node, cur_node + 1, result + node[cur_node], result_list)
    dfs(node, cur_node + 1, result - node[cur_node], result_list)
    # 결과값 리스트를 반환
    return result_list


if __name__ == "__main__":
    print(solution([1, 1, 1, 1, 1], 3))
~~~
