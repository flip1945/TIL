~~~python
def solution(string):
    stack = []

    for s in string:
        if stack:
            stack = stack_chk(stack, s)
        else:
            if s == ")":
                return "NO"
            else:
                stack.append(s)
    return is_empty(stack)


def stack_chk(stack, s):
    if valid_ps_chk(stack[-1], s):
        return stack[:-1]   # stack.pop()
    return stack + [s]      # stack.append(s)


def valid_ps_chk(top, cur):
    if top == cur:
        return False
    return True


def is_empty(stack):
    if stack:
        return "NO"
    return "YES"


if __name__ == "__main__":
    for T in range(int(input())):
        print(solution(input()))
~~~
