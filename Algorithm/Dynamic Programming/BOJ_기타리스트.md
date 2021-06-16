~~~python
from collections import deque

n, s, m = map(int, input().split())
v = list(map(int, input().split()))

dp = [[-1] * (m+1) for _ in range(n)]
que = deque([(0, s)])

while que:
    i, cur = que.popleft()
    
    for ne in [cur-v[i], cur+v[i]]:
        if 0<=ne<=m and dp[i][ne] == -1:
            dp[i][ne] = ne
            if i < n - 1:
                que.append((i+1, ne))

print(max(dp[-1]))
~~~
