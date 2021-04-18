~~~python
from collections import deque


def solution(priorities, location):
    answer = 0
    que = deque(priorities)
    que[location] = float(que[location])

    while len(que) > 1:
        if que_pop_chk(list(que)):
            if type(que.popleft()) == float:
                return answer + 1
            answer += 1
        else:
            que.rotate(-1)

    return answer + 1


def str_to_list(string):
    return [int(i) for i in string.split()]


def que_pop_chk(que):
    if que[0] >= max(que[1:]):
        return True
    return False


if __name__ == "__main__":
    for T in range(int(input())):
        n, c = map(int, input().split())
        print(solution(str_to_list(input()), c))
~~~
