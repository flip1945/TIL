# 유레카 이론 (Bronze 2)

### 문제 설명

삼각수 Tn(n ≥ 1)는 [그림]에서와 같이 기하학적으로 일정한 모양의 규칙을 갖는 점들의 모음으로 표현될 수 있다.

<p align="center">
    <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images2/eureka.png">
</p>

자연수 n에 대해 n ≥ 1의 삼각수Tn는 명백한 공식이 있다.   

Tn = 1 + 2 + 3 + ... + n = n(n+1)/2   

1796년, 가우스는 모든 자연수가 최대 3개의 삼각수의 합으로 표현될 수 있다고 증명하였다. 예를 들어,   

* 4 = T1 + T2

* 5 = T1 + T1 + T2

* 6 = T2 + T2 or 6 = T3

* 10 = T1 + T2 + T3 or 10 = T4

이 결과는 증명을 기념하기 위해 그의 다이어리에 “Eureka! num = Δ + Δ + Δ” 라고 적은것에서 유레카 이론으로 알려졌다. 꿍은 몇몇 자연수가 정확히 3개의 삼각수의 합으로 표현될 수 있는지 궁금해졌다. 위의 예시에서, 5와 10은 정확히 3개의 삼각수의 합으로 표현될 수 있지만 4와 6은 그렇지 않다.   

자연수가 주어졌을 때, 그 정수가 정확히 3개의 삼각수의 합으로 표현될 수 있는지 없는지를 판단해주는 프로그램을 만들어라. 단, 3개의 삼각수가 모두 달라야 할 필요는 없다.   

---

#### 입력

프로그램은 표준입력을 사용한다. 테스트케이스의 개수는 입력의 첫 번째 줄에 주어진다. 각 테스트케이스는 한 줄에 자연수 K (3 ≤ K ≤ 1,000)가 하나씩 포함되어있는 T개의 라인으로 구성되어있다.

---

#### 출력

프로그램은 표준출력을 사용한다. 각 테스트케이스에대해 정확히 한 라인을 출력한다. 만약 K가 정확히 3개의 삼각수의 합으로 표현될수 있다면 1을, 그렇지 않다면 0을 출력한다.

---
#### 예제 입력 1

~~~
3
10
20
1000
~~~

#### 예제 출력 1

~~~
1
0
1
~~~

---

출처 : https://www.acmicpc.net/problem/10448

---

### 문제풀이

이번 문제는 완전 탐색의 기초 문제입니다.   

이 문제에서 1000이하를 만족하는 삼각수n은 45개 이므로 O(N^3)의 알고리즘으로도 문제를 해결할 수 있습니다.   

45가지를 모두 확인해보면 정답을 파악할 수 있습니다.   

---

#### 나의 풀이

~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	// 삼각수를 저장할 배열
    	ArrayList<Integer> t = new ArrayList<Integer>();
    	int sum = 0;
    	int n = 1;
    	
        // 삼각수를 구하는 부분
    	while (true) {
    		sum = (int)(n * (n + 1) / 2);
    		if (sum >= 1000) break;
    		t.add(sum);
    		n++;
    	}
        // 반복횟수를 입력
    	int count = Integer.parseInt(br.readLine());
    	
    	for (int testCase = 0; testCase < count; testCase++) {
    		int testCaseNum = Integer.parseInt(br.readLine());
    		System.out.println(isEureka(t, testCaseNum) ? 1 : 0);
    	}
    }
    // 유레카 이론이 맞는지 확인하는 메소드
    private static boolean isEureka(ArrayList<Integer> t, int testCaseNum) {
    	for (int i = 0; i < t.size(); i++) {
    		for (int j = 0; j < t.size(); j++) {
    			for (int k = 0; k < t.size(); k++) {
    				if (t.get(i) + t.get(j) + t.get(k) == testCaseNum) {
    					return true;
    				}
    			}
    		}
    	}
    	return false;
    }
}
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/27648157

~~~java
import java.util.Scanner;

public class Main {
	public static void main(String[] args){
		int[] arr = new int[45];
		boolean[] answer = new boolean[5000];
		
		Scanner sc = new Scanner(System.in);
		int tc = sc.nextInt();			

		for(int i=1;i<=arr.length-1;i++) {
			arr[i] = i*(i+1)/2;
		}
		for(int j=1;j<=arr.length-1;j++) {
			for(int k=1;k<=arr.length-1;k++) {
				for(int l=1;l<=arr.length-1;l++) {
					answer[arr[j]+arr[k]+arr[l]]=true;
				}
			}
		}
		for(int i=0;i<tc;i++) {
			int num = sc.nextInt();
			System.out.println(answer[num]?"1":"0");
		}
	}
}
~~~
