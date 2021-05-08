# 길 찾기 게임(Level 3)

### 문제 설명

전무로 승진한 라이언은 기분이 너무 좋아 프렌즈를 이끌고 특별 휴가를 가기로 했다.   
내친김에 여행 계획까지 구상하던 라이언은 재미있는 게임을 생각해냈고 역시 전무로 승진할만한 인재라고 스스로에게 감탄했다.   

라이언이 구상한(그리고 아마도 라이언만 즐거울만한) 게임은, 카카오 프렌즈를 두 팀으로 나누고, 각 팀이 같은 곳을 다른 순서로 방문하도록 해서 먼저 순회를 마친 팀이 승리하는 것이다.   

그냥 지도를 주고 게임을 시작하면 재미가 덜해지므로, 라이언은 방문할 곳의 2차원 좌표 값을 구하고 각 장소를 이진트리의 노드가 되도록 구성한 후, 순회 방법을 힌트로 주어 각 팀이 스스로 경로를 찾도록 할 계획이다.   

라이언은 아래와 같은 특별한 규칙으로 트리 노드들을 구성한다.   

* 트리를 구성하는 모든 노드의 x, y 좌표 값은 정수이다.
* 모든 노드는 서로 다른 x값을 가진다.
* 같은 레벨(level)에 있는 노드는 같은 y 좌표를 가진다.
* 자식 노드의 y 값은 항상 부모 노드보다 작다.
* 임의의 노드 V의 왼쪽 서브 트리(left subtree)에 있는 모든 노드의 x값은 V의 x값보다 작다.
* 임의의 노드 V의 오른쪽 서브 트리(right subtree)에 있는 모든 노드의 x값은 V의 x값보다 크다.

아래 예시를 확인해보자.

라이언의 규칙에 맞게 이진트리의 노드만 좌표 평면에 그리면 다음과 같다. (이진트리의 각 노드에는 1부터 N까지 순서대로 번호가 붙어있다.)

<img src = "https://grepp-programmers.s3.amazonaws.com/files/production/dbb58728bd/a5371669-54d4-42a1-9e5e-7466f2d7b683.jpg">

이제, 노드를 잇는 간선(edge)을 모두 그리면 아래와 같은 모양이 된다.   

<img src = https://grepp-programmers.s3.amazonaws.com/files/production/6bd8f6496a/50e1df20-5cb7-4846-86d6-2a2f1e70c5da.jpg>

위 이진트리에서 전위 순회(preorder), 후위 순회(postorder)를 한 결과는 다음과 같고, 이것은 각 팀이 방문해야 할 순서를 의미한다.

* 전위 순회 : 7, 4, 6, 9, 1, 8, 5, 2, 3
* 후위 순회 : 9, 6, 5, 8, 1, 4, 3, 2, 7

다행히 두 팀 모두 머리를 모아 분석한 끝에 라이언의 의도를 간신히 알아차렸다.   

그러나 여전히 문제는 남아있다. 노드의 수가 예시처럼 적다면 쉽게 해결할 수 있겠지만, 예상대로 라이언은 그렇게 할 생각이 전혀 없었다.   

이제 당신이 나설 때가 되었다.   

곤경에 빠진 카카오 프렌즈를 위해 이진트리를 구성하는 노드들의 좌표가 담긴 배열 nodeinfo가 매개변수로 주어질 때,   
노드들로 구성된 이진트리를 전위 순회, 후위 순회한 결과를 2차원 배열에 순서대로 담아 return 하도록 solution 함수를 완성하자.   

---

#### 제한사항

* nodeinfo는 이진트리를 구성하는 각 노드의 좌표가 1번 노드부터 순서대로 들어있는 2차원 배열이다.
    * nodeinfo의 길이는 1 이상 10,000 이하이다.
    * nodeinfo[i] 는 i + 1번 노드의 좌표이며, [x축 좌표, y축 좌표] 순으로 들어있다.
    * 모든 노드의 좌표 값은 0 이상 100,000 이하인 정수이다.
    * 트리의 깊이가 1,000 이하인 경우만 입력으로 주어진다.
    * 모든 노드의 좌표는 문제에 주어진 규칙을 따르며, 잘못된 노드 위치가 주어지는 경우는 없다.

---

#### 입출력 예
|nodeinfo|	result|
|-|-|
|[[5,3],[11,5],[13,3],[3,5],[6,1],[1,3],[8,6],[7,2],[2,2]]|	[[7,4,6,9,1,8,5,2,3],[9,6,5,8,1,4,3,2,7]]|

---

#### 입출력 예 설명

##### 입출력 예 #1

문제에 주어진 예시와 같다.   

[출처](https://programmers.co.kr/learn/courses/30/lessons/42892)

---

### 문제풀이



---

#### 나의 풀이
~~~python
import sys
sys.setrecursionlimit(10**6)



def solution(nodeinfo):
    answer = []

    label_nodes = label(nodeinfo)
    root_index = get_root(label_nodes)
    result_list = [[] for i in range(len(nodeinfo))]

    tree = make_tree(root_index, label_nodes, result_list)

    # 입력값이 1개면 트리가 생성되지 않으므로 예외처리
    if tree is None:
        return [[1], [1]]
    # 결과에 전위 순회 값을 저장
    answer.append(preorder_dfs(tree, root_index, []))
    # 결과에 후위 순회 값을 저장
    answer.append(postorder_dfs(tree, root_index, []))

    return answer

# nodes의 x값을 기준으로 왼쪽, 오른쪽 딕셔너리를 생성하는 함수
def get_side_list(root_index, nodes):
    left_list = {}
    right_list = {}

    for i in nodes.keys():
        # 왼쪽 트리 딕셔너리 생성
        if nodes[i][0] < nodes[root_index][0]:
            left_list[i] = nodes[i]
        # 오른쪽 트리 딕셔너리 생성
        elif nodes[i][0] > nodes[root_index][0]:
            right_list[i] = nodes[i]

    return left_list, right_list

# nodes의 루트 노드를 반환하는 함수
def get_root(nodes):
    m = -1
    root_index = 0
    for i in nodes.keys():
        if nodes[i][1] > m:
            root_index = i
            m = nodes[i][1]
    return root_index

# 재귀적으로 트리를 만드는 함수
def make_tree(root_index, nodes, result_list):
    # 노드의 갯수가 자기 자신 1개 뿐이라면 종료
    if len(nodes) == 1:
        return
    # 현재 노드를 기준으로 왼쪽, 오른쪽 딕셔너리 생성
    left_list, right_list = get_side_list(root_index, nodes)
    # 현재 노드보다 x값이 적은 딕셔너리 존재한다면
    if left_list:
        # 왼쪽 루트 노드를 정함
        left_root_index = get_root(left_list)
        # result_list에 만든 트리 정보를 저장
        result_list[root_index].append(left_root_index)
        # 왼쪽 딕셔너리로 구성된 트리를 재귀로 생성
        make_tree(left_root_index, left_list, result_list)
    # 현재 노드보다 x값이 큰 딕셔너리가 존재한다면
    if right_list:
        # 오른쪽 루트 노드를 정함
        right_root_index = get_root(right_list)
        # result_list에 만든 트리 정보를 저장
        result_list[root_index].append(right_root_index)
        # 오른쪽 딕셔너리로 구성된 트리를 재귀로 생성
        make_tree(right_root_index, right_list, result_list)
    # 결과 리스트를 반환
    return result_list

# 리스트를 번호가 있는 딕셔너리로 저장하는 함수
def label(nodes):
    dic = {}
    for i in range(len(nodes)):
        dic[i] = nodes[i]
    return dic

# 전위 순회
def preorder_dfs(tree, node, result):
    result.append(node + 1)
    for n in tree[node]:
        preorder_dfs(tree, n, result)
    return result

# 후위 순회
def postorder_dfs(tree, node, result):
    for n in tree[node]:
        postorder_dfs(tree, n, result)
    result.append(node + 1)
    return result
~~~

---

#### 다른 사람의 풀이

출처 : https://minnnne.tistory.com/100

~~~python
import sys
sys.setrecursionlimit(10**6)

class Tree:
    def __init__(self):
        self.parent = None
        self.left = None
        self.right = None
        self.data = None
        self.index = None

def preOrder(root, vector):
    if root == None:
        return vector
    
    vector.append(root.index)
    preOrder(root.left,vector)
    preOrder(root.right,vector)
    
    return vector

def postOrder(root, vector):

    if root == None:
        return vector
    postOrder(root.left, vector)
    postOrder(root.right, vector)
    vector.append(root.index)

    return vector


def solution(nodeinfo):
    root = None
    for i in range(len(nodeinfo)):
        nodeinfo[i].append(i+1)
    nodeinfo = sorted(nodeinfo, key= lambda x:x[1], reverse=True)
    
    for i,node in enumerate(nodeinfo):
        newTree = Tree()
        newTree.index = node[2]
        newTree.data = node
        if root == None:
            root = newTree
        else:
            curTree = root
            while 1:
                if curTree.data[0] < newTree.data[0]: # 오른쪽
                    if curTree.right == None:
                        curTree.right = newTree
                        newTree.parent = curTree
                        break
                    else:
                        curTree = curTree.right
                else: #왼쪽
                    if curTree.left == None:
                        curTree.left = newTree
                        newTree.parent = curTree
                        break
                    else:
                        curTree = curTree.left
    
    answer = [preOrder(root,[]), postOrder(root,[])]
    return answer
~~~
