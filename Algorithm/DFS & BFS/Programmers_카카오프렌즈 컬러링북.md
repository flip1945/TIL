# 카카오프렌즈 컬러링북(Level 2)

### 문제 설명

출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)   

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.   

<img src = "http://t1.kakaocdn.net/codefestival/apeach.png">

위의 그림은 총 12개 영역으로 이루어져 있으며, 가장 넓은 영역은 어피치의 얼굴면으로 넓이는 120이다.   

---

#### 입력 형식

입력은 그림의 크기를 나타내는 m과 n, 그리고 그림을 나타내는 m × n 크기의 2차원 배열 picture로 주어진다. 제한조건은 아래와 같다.   

* 1 <= m, n <= 100

* picture의 원소는 0 이상 2^31 - 1 이하의 임의의 값이다.

* picture의 원소 중 값이 0인 경우는 색칠하지 않는 영역을 뜻한다.

#### 출력 형식

리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.   

---

#### 예제 입출력

|m|	n|	picture|	answer|
|-|-|-|-|
|6|	4|	\[\[1, 1, 1, 0], \[1, 2, 2, 0], \[1, 0, 0, 1], \[0, 0, 0, 1], \[0, 0, 0, 3], \[0, 0, 0, 3]]|	\[4, 5]|

#### 예제에 대한 설명

예제로 주어진 그림은 총 4개의 영역으로 구성되어 있으며, 왼쪽 위의 영역과 오른쪽의 영역은 모두 1로 구성되어 있지만 상하좌우로 이어져있지 않으므로 다른 영역이다. 가장 넓은 영역은 왼쪽 위 1이 차지하는 영역으로 총 5칸이다.   

[출처](https://programmers.co.kr/learn/courses/30/lessons/1829)

---

### 문제풀이

이 문제는 탐색을 사용해서 각 영역의 크기를 구하는 문제입니다.   

이 문제는 java와 c++로만 풀 수 있어서 java로 문제를 풀었습니다.   

계속 python으로 문제를 풀다가 java로 문제를 풀려고 하니까 조금 신선하기도 했고,   
java로 문제를 푸는 건 처음이라 좀 해맸습니다.   

왜 알고리즘 문제 풀이에서 python을 추천하는 지 그 이유를 알게 됐습니다.   

java로 문제를 푸니 code line수가 pyhton 보다 많아서 coding하는 데 힘들었습니다.   

문제 풀이로 넘어가면, 이 문제는 dfs나 bfs를 통해 특정 영역의 크기를 구하고, 영역의 개수를 구하는 문제입니다.   

저는 bfs를 이용했고, 가장 큰 영역을 저장하는 변수와 영역의 개수를 저장하는 변수를 사용해서 문제를 풀었습니다.   

1. 먼저 모든 좌표를 탐색하도록 반복문을 짜고, (0, 0) 좌표부터 탐색을 시작합니다.
2. 탐색을 통해 칠해야 하는 색이 같고 상하좌우로 이어진 좌표들은 방문처리를 합니다.
3. 더 이상 탐색할 수 없으면 가장 큰 영역의 크기를 저장하는 변수와 현재 영역의 크기를 비교하고, 더 큰 값을 가장 큰 영역을 저장하는 변수에 넣습니다.
4. 그리고 영역의 개수를 저장하는 변수의 값을 1 증가시킵니다.
5. 다음 좌표를 탐색하는데, 이미 방문한 좌표는 탐색하지 않습니다.
6. 마지막 좌표까지 2~5를 반복합니다.

---

#### 나의 풀이

~~~java
import java.util.*;

class Solution {
    public int[] solution(int m, int n, int[][] picture) {
        Queue<int[]> queue = new LinkedList<int[]>();
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;
        boolean visited[][] = new boolean[m][n];
        int[] wayx = {0, 0, 1, -1};
        int[] wayy = {1, -1, 0, 0};
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (picture[i][j] == 0) {
                    visited[i][j] = true;
                }
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (visited[i][j]) {
                    continue;
                }
                
                int[] temp = {i, j, 1};
                int type = picture[i][j];
                int areaSize = 1;
                queue.offer(temp);
                visited[i][j] = true;
                
                numberOfArea++;
                
                while (queue.size() > 0) {
                    int[] cur = queue.poll();
                    int x = cur[0];
                    int y = cur[1];

                    for (int k = 0; k < 4; k++) {
                        int dx = x + wayx[k];
                        int dy = y + wayy[k];

                        if (dx < 0 || dx >= m || dy < 0 || dy >= n ||
                            visited[dx][dy] || type != picture[dx][dy]) {
                            continue;
                        }
                        int[] nextTemp = {dx, dy};
                        queue.offer(nextTemp);
                        visited[dx][dy] = true;

                        if (++areaSize > maxSizeOfOneArea) {
                            maxSizeOfOneArea = areaSize;
                        }
                    }
                }
            }
        }
        
        int[] answer = new int[2];
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;
        return answer;
    }
}
~~~

---

#### 다른 사람의 풀이

~~~java
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    public int[] solution(int m, int n, int[][] picture) {

        boolean[][] check = new boolean[m][n];
        Queue<Node> q = new LinkedList<>();
        int[] dx = { 0, 0, -1, 1 };
        int[] dy = { 1, -1, 0, 0 };

        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;

        for (int i = 0; i < picture.length; i++) {
            for (int j = 0; j < picture[i].length; j++) {
                int tempMax = 0;
                if (!check[i][j] && picture[i][j] != 0) {
                    check[i][j] = true;
                    q.add(new Node(i, j));
                    tempMax++;

                    while (!q.isEmpty()) {

                        Node current = q.poll();

                        int nextX = 0;
                        int nextY = 0;

                        for (int k = 0; k < 4; k++) {
                            nextX = current.x + dx[k];
                            nextY = current.y + dy[k];

                            if (nextX < 0 || nextY < 0 || nextX >= m || nextY >= n || check[nextX][nextY])
                                continue;
                            if (picture[current.x][current.y] == picture[nextX][nextY]) {
                                check[nextX][nextY] = true;
                                q.add(new Node(nextX, nextY));
                                tempMax++;
                            }
                        } // end of for_k

                    } // end of while
                    numberOfArea++;
                    maxSizeOfOneArea = maxSizeOfOneArea < tempMax ? tempMax : maxSizeOfOneArea;
                } // end of if

            } // end of for_j
        } // end of for_i

        int[] answer = new int[2];
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;

        System.out.println(numberOfArea + "," + maxSizeOfOneArea);

        return answer;

    } // end of solution

    static class Node {
        int x, y;

        public Node(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
~~~
