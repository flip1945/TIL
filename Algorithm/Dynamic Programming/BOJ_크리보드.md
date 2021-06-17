~~~python
n = int(input())

dp = [i for i in range(1, n+1)]

for i in range(1, n):
    for j in range(i+3, n):
        dp[j] = max(dp[j], dp[i]*(j-i-1))
print(dp[-1])
~~~
