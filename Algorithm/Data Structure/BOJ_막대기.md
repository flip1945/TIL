# 막대기 (Bronze 2)

### 문제

아래 그림처럼 높이만 다르고 (같은 높이의 막대기가 있을 수 있음) 모양이 같은 막대기를 일렬로 세운 후, 왼쪽부터 차례로 번호를 붙인다. 각 막대기의 높이는 그림에서 보인 것처럼 순서대로 6, 9, 7, 6, 4, 6 이다. 일렬로 세워진 막대기를 오른쪽에서 보면 보이는 막대기가 있고 보이지 않는 막대기가 있다. 즉, 지금 보이는 막대기보다 뒤에 있고 높이가 높은 것이 보이게 된다. 예를 들어, 그림과 같은 경우엔 3개(6번, 3번, 2번)의 막대기가 보인다.

<p align="center">
    <img src="https://upload.acmicpc.net/a2ebef22-157f-4059-9bdd-6a0662b81698/-/crop/675x304/47,12/-/preview/" width=300>
</p>

N개의 막대기에 대한 높이 정보가 주어질 때, 오른쪽에서 보아서 몇 개가 보이는지를 알아내는 프로그램을 작성하려고 한다.

---

#### 입력

첫 번째 줄에는 막대기의 개수를 나타내는 정수 N (2 ≤ N ≤ 100,000)이 주어지고 이어지는 N줄 각각에는 막대기의 높이를 나타내는 정수 h(1 ≤ h ≤ 100,000)가 주어진다.

---

#### 출력

오른쪽에서 N개의 막대기를 보았을 때, 보이는 막대기의 개수를 출력한다.

---

#### 예제 입력 1
~~~
6
6
9
7
6
4
6
~~~

#### 예제 출력 1
~~~
3
~~~

#### 예제 입력 2
~~~
5
5
4
3
2
1
~~~

#### 예제 출력 2
~~~
5
~~~

[출처](https://www.acmicpc.net/problem/17608)

---

### 문제풀이

이 문제는 stack을 이용해서 풀 수 있는 문제입니다.   

막대기를 stack에 저장하고, pop연산을 통해 뒤에서부터 막대기의 크기를 확인할 수 있습니다.   

뒤에서 확인할 때, 굳이 stack을 사용하지 않아도 되지만 연습을 위해서 stack을 이용해 문제를 풀어봤습니다.   

뒤에서부터 막대기의 크기를 확인하면서 현재까지 확인한 막대기보다 더 큰 막대기가 나올때만 정답값을 1씩 증가시켜줍니다.   

이렇게 하면 한번에 확인할 수 있는 막대기의 개수를 알 수 있게 됩니다.

---

#### 나의 풀이

~~~python
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        Stack<Integer> stack = new Stack<>();
        int answer = 0;
        int height = 0;
        int n = Integer.parseInt(br.readLine());
		
        for (int i = 0; i < n; i++) {
            stack.push(Integer.parseInt(br.readLine()));
        }
		
        while (!stack.empty()) {
            if (stack.peek() > height) {
                answer++;
            }
            height = Math.max(height, stack.pop());
        }
		
        System.out.println(answer);
	}
}
~~~
