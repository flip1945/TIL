~~~python
from collections import deque


def solution(n, k):
    result = []
    que = deque([i + 1 for i in range(n)])

    while que:
        que.rotate(-k+1)
        result.append(str(que.popleft()))

    return print_answer(result)


def print_answer(answer):
    return "<" + ", ".join(answer) + ">"


if __name__ == "__main__":
    print(solution(5000, 3))
    print(solution(7, 3))
    print(solution(5000, 4999))
~~~
