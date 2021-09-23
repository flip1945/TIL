# 분해합 (Bronze 2)

### 문제 설명

어떤 자연수 N이 있을 때, 그 자연수 N의 분해합은 N과 N을 이루는 각 자리수의 합을 의미한다. 어떤 자연수 M의 분해합이 N인 경우, M을 N의 생성자라 한다. 예를 들어, 245의 분해합은 256(=245+2+4+5)이 된다. 따라서 245는 256의 생성자가 된다. 물론, 어떤 자연수의 경우에는 생성자가 없을 수도 있다. 반대로, 생성자가 여러 개인 자연수도 있을 수 있다.   

자연수 N이 주어졌을 때, N의 가장 작은 생성자를 구해내는 프로그램을 작성하시오.   

---

#### 입력

첫째 줄에 자연수 N(1 ≤ N ≤ 1,000,000)이 주어진다.

---

#### 출력

첫째 줄에 답을 출력한다. 생성자가 없는 경우에는 0을 출력한다.

---
#### 예제 입력 1

~~~
216
~~~

#### 예제 출력 1

~~~
198
~~~

---

출처 : https://www.acmicpc.net/problem/2231

---

### 문제풀이

이번 문제는 완전 탐색의 기초 문제입니다.   

N의 가장 작은 생성자를 구하는 문제인데, N의 크기가 100만 이므로 O(N)의 알고리즘으로 충분히 해결할 수 있습니다.   

그러므로 N이 주어지만 1부터 N까지의 수 중에 생성자가 있는지 확인하면 됩니다.   

---

#### 나의 풀이

~~~java
import java.*;

public class Main {
    public static void main(String[] args) throws IOException {
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	
    	int n = Integer.parseInt(br.readLine());
    	
    	for (int i = 1; i < n; i++) {
            // 숫자를 문자열로 변환
    		String num = Integer.toString(i);
            // 문자열을 문자열 배열로 변환
    		String[] numArray = num.split("");
    		int sum = i;
            // 모든 배열을 돌면서 합을 더함
    		for (int j = 0; j < numArray.length; j++) {
    			sum += Integer.parseInt(numArray[j]);
    		}
            // N의 생성자 인지 확인
    		if (sum == n) {
    			System.out.println(i);
    			return;
    		}
    	}
    	System.out.println(0);
    }
}
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/30759226

~~~java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		int n = new Scanner(System.in).nextInt(), i = 1, dn;
		do {
			dn = i;
			for (int t = i; t > 0; dn += t % 10, t /= 10)
				;
		} while(dn != n && i++ < n);
		System.out.print(i > n ? 0 : i);
	}
}
~~~
