# 단체사진 찍기(Level 2)

### 문제 설명

<img src = "https://t1.kakaocdn.net/codefestival/picture.png">

가을을 맞아 카카오프렌즈는 단체로 소풍을 떠났다. 즐거운 시간을 보내고 마지막에 단체사진을 찍기 위해 카메라 앞에 일렬로 나란히 섰다. 그런데 각자가 원하는 배치가 모두 달라 어떤 순서로 설지 정하는데 시간이 오래 걸렸다. 네오는 프로도와 나란히 서기를 원했고, 튜브가 뿜은 불을 맞은 적이 있던 라이언은 튜브에게서 적어도 세 칸 이상 떨어져서 서기를 원했다. 사진을 찍고 나서 돌아오는 길에, 무지는 모두가 원하는 조건을 만족하면서도 다르게 서는 방법이 있지 않았을까 생각해보게 되었다. 각 프렌즈가 원하는 조건을 입력으로 받았을 때 모든 조건을 만족할 수 있도록 서는 경우의 수를 계산하는 프로그램을 작성해보자.   

---

#### 입력 형식

입력은 조건의 개수를 나타내는 정수 n과 n개의 원소로 구성된 문자열 배열 data로 주어진다. data의 원소는 각 프렌즈가 원하는 조건이 N~F=0과 같은 형태의 문자열로 구성되어 있다. 제한조건은 아래와 같다.   

* 1 <= n <= 100

* data의 원소는 다섯 글자로 구성된 문자열이다. 각 원소의 조건은 다음과 같다.
    * 첫 번째 글자와 세 번째 글자는 다음 8개 중 하나이다. {A, C, F, J, M, N, R, T} 각각 어피치, 콘, 프로도, 제이지, 무지, 네오, 라이언, 튜브를 의미한다. 첫 번째 글자는 조건을 제시한 프렌즈, 세 번째 글자는 상대방이다. 첫 번째 글자와 세 번째 글자는 항상 다르다.
    * 두 번째 글자는 항상 ~이다.
    * 네 번째 글자는 다음 3개 중 하나이다. {=, <, >} 각각 같음, 미만, 초과를 의미한다.
    * 다섯 번째 글자는 0 이상 6 이하의 정수의 문자형이며, 조건에 제시되는 간격을 의미한다. 이때 간격은 두 프렌즈 사이에 있는 다른 프렌즈의 수이다.

#### 출력 형식

모든 조건을 만족하는 경우의 수를 리턴한다.   

---

#### 예제 입출력

|n|	data|	answer|
|-|-|-|
|2|	\["N\~F=0", "R\~T>2"]|	3648|
|2|	\["M\~C<2", "C\~M>1"]|	0|

#### 예제에 대한 설명

첫 번째 예제는 문제에 설명된 바와 같이, 네오는 프로도와의 간격이 0이기를 원하고 라이언은 튜브와의 간격이 2보다 크기를 원하는 상황이다.   

두 번째 예제는 무지가 콘과의 간격이 2보다 작기를 원하고, 반대로 콘은 무지와의 간격이 1보다 크기를 원하는 상황이다. 이는 동시에 만족할 수 없는 조건이므로 경우의 수는 0이다.   

[출처](https://programmers.co.kr/learn/courses/30/lessons/1835)

---

### 문제풀이

이번 문제는 예전 문제라서 python을 사용하지 못하고, java 혹은 c++을 사용해서 물제를 풀어야 합니다.   

저는 java로 문제를 풀었습니다.   

문제를 풀면서 'python 이라면 내장함수를 쓰면 편한 데'라는 생각이 들었습니다.   

이 문제를 풀기 위해서는 8명의 순열을 구해야 하는데, java에서는 permutation 함수가 없어서 직접 만들어서 문제를 풀었습니다.   

permutation 함수는 백트래킹을 이용해 만들었습니다.   

이 문제를 해결하는 방법은 완전 탐색을 이용해 푸는 것입니다.   

8명으로 할 수 있는 모든 순열을 구하고, 각 순열이 모든 조건을 만족하는 지 파악하면 됩니다.   

---

#### 나의 풀이

~~~java
import java.util.*;

class Solution {
    public int solution(int n, String[] data) {
        int answer = 0;
        String[] friends = { "A", "C", "F", "J", "M", "N", "R", "T" };
        ArrayList<StringBuilder> arrayList = new ArrayList<StringBuilder>();
        
        arrayList = permutation(arrayList, friends, new StringBuilder(), new boolean[8]);
        
        for (StringBuilder sb : arrayList) {
            boolean allCheck = true;
            
            for (int i = 0; i < n; i++) {
                Data newData = new Data(data[i]);
                int diff = Math.abs(sb.indexOf(newData.p1) - sb.indexOf(newData.p2)) - 1;
                
                if (newData.op.equals("<")) {
                    if (!(diff < newData.space)) {
                        allCheck = false;
                        break;
                    }
                } else if (newData.op.equals(">")) {
                    if (!(diff > newData.space)) {
                        allCheck = false;
                        break;
                    }
                } else {
                    if (!(diff == newData.space)) {
                        allCheck = false;
                        break;
                    }
                }
            }
            
            if (allCheck) {
                answer++;
            }
        }
        return answer;
    }
    
    public ArrayList<StringBuilder> permutation(ArrayList al, String[] friends, StringBuilder sb, boolean[] visited) {        
        if (sb.length() == 8) {
            al.add(new StringBuilder(sb));
            return al;
        }
        
        for (int i = 0; i < 8; i++) {
            if (visited[i]) {
                continue;
            }
            
            sb.append(friends[i]);
            visited[i] = true;
            permutation(al, friends, sb, visited);
            sb.setLength(sb.length() - 1);
            visited[i] = false;
        }
        return al;
    }
}

class Data {
    String p1;
    String p2;
    String op;
    int space;
    
    public Data(String data) {
        p1 = data.substring(0,1);
        p2 = data.substring(2,3);
        op = data.substring(3,4);
        space = Integer.parseInt(data.substring(4,5));
    }
}
~~~

#### 다른 사람의 풀이

~~~java
import java.util.*;
class Solution {
    static String[] arr = {"A","C","F","J","M","N","R","T"};
    static String[] result = new String[8];
    static boolean[] used = new boolean[8];
    static int answer;
    public int solution(int n, String[] data) {
        answer = 0;
        perm(0,data);
        return answer;
    }
    static void perm(int cnt, String[] data){
        if(cnt==8){
            String s = "";
            for(int i=0; i<arr.length; i++){
                s+=result[i];
            }
            for(int i=0; i<data.length; i++){
                int start = s.indexOf(data[i].charAt(0));
                int end = s.indexOf(data[i].charAt(2));

                if(data[i].charAt(3)=='=' && Math.abs(start-end)-1!=data[i].charAt(4)-'0'){
                    return;
                }else if(data[i].charAt(3)=='<' && Math.abs(start-end)-1>=data[i].charAt(4)-'0'){
                    return;
                }else if(data[i].charAt(3)=='>' && Math.abs(start-end)-1<=data[i].charAt(4)-'0'){
                    return;
                }
            }
            answer++;
            return;
        }
        for(int i=0; i<arr.length; i++){
            if(!used[i]){
                used[i] = true;
                result[cnt] = arr[i];
                perm(cnt+1, data);
                used[i] = false;
            }
        }
    }
}
~~~
