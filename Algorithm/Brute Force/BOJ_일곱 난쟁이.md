# 일곱 난쟁이(Bronze 2)

### 문제 설명

왕비를 피해 일곱 난쟁이들과 함께 평화롭게 생활하고 있던 백설공주에게 위기가 찾아왔다. 일과를 마치고 돌아온 난쟁이가 일곱 명이 아닌 아홉 명이었던 것이다.   

아홉 명의 난쟁이는 모두 자신이 "백설 공주와 일곱 난쟁이"의 주인공이라고 주장했다. 뛰어난 수학적 직관력을 가지고 있던 백설공주는, 다행스럽게도 일곱 난쟁이의 키의 합이 100이 됨을 기억해 냈다.   

아홉 난쟁이의 키가 주어졌을 때, 백설공주를 도와 일곱 난쟁이를 찾는 프로그램을 작성하시오.   

---

#### 입력

아홉 개의 줄에 걸쳐 난쟁이들의 키가 주어진다. 주어지는 키는 100을 넘지 않는 자연수이며, 아홉 난쟁이의 키는 모두 다르며, 가능한 정답이 여러 가지인 경우에는 아무거나 출력한다.

---

#### 출력

일곱 난쟁이의 키를 오름차순으로 출력한다. 일곱 난쟁이를 찾을 수 없는 경우는 없다.

---
#### 예제 입력 1

~~~
20
7
23
19
10
15
25
8
13
~~~

#### 예제 출력 1

~~~
7
8
10
13
19
20
23
~~~

---

[출처](https://www.acmicpc.net/problem/2309)

---

### 문제풀이

이번 문제는 완전 탐색의 기초 문제입니다.   

푸는 방법이 여러가지 있지만 저는 selected 배열을 만들어서 문제를 풀었습니다.   

selected 배열이 true인 난쟁이를 2명 선택하고 선택하지 않은 나머지 7명의 난쟁이의 키를 모두 더해서 100이 되는지 확인하도록 했습니다.   

다른 방식으로 문제를 해결한 분의 코드를 아래에 첨부했습니다.

---

#### 나의 풀이

~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        boolean[] selected = new boolean[9];
        // 난쟁이들의 키를 저장할 배열
        ArrayList<Integer> heights = new ArrayList<Integer>();
        // 난쟁이들의 키를 입력
        for (int i = 0; i < 9; i++) {
        	heights.add(Integer.parseInt(br.readLine()));
        }
        // 오름차순 정렬
        Collections.sort(heights);
        // 모든 경우의 수를 탐색
        for (int i = 0; i < 9; i++) {
        	selected[i] = true;
            for (int j = i + 1; j < 9; j++) {
            	selected[j] = true;
              // 선택하지 않은 난쟁이들의 키의 합이 100인 경우 종료
            	if (notSelectedSum(selected, heights) == 100) {
            		printHeights(selected, heights);
            		return;
            	}
                selected[j] = false;
            }
            selected[i] = false;
        }
    }
    // 선택하지 않은 난쟁이들의 키를 모두 더하는 메소드
    private static int notSelectedSum(boolean[] selected, ArrayList<Integer> array) {
    	int sum = 0;
    	for (int i = 0; i < array.size(); i++) {
    		if (!selected[i])
    		sum += array.get(i);
    	}
    	return sum;
    }
    // 난쟁이들의 키를 출력하는 메소드
    private static void printHeights(boolean[] selected, ArrayList<Integer> array) {
    	for (int i = 0; i < array.size(); i++) {
    		if (!selected[i]) {
    			System.out.println(array.get(i));
    		}
    	}
    }
}
~~~

---

#### 다른 사람의 풀이

~~~java
import java.util.*;

public class Main {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		
		int sum=0, i=0, j=0;
		int arr[] = new int[9];
		
		for(i=0; i<9; i++) {
			arr[i] = scan.nextInt();
			sum += arr[i];
		}
		
		for(i=0; i<9; i++) {
			for(j=i+1; j<9; j++) {
				if(sum-arr[i]-arr[j] == 100) {
					arr[i] = -1;
					arr[j] = -1;
					break;
				}
			}
			if(arr[i] <0 && arr[j] <0)
				break;
		}
		
		Arrays.sort(arr);
		for(i=2; i<9; i++)
			System.out.println(arr[i]);
	}
}
~~~
