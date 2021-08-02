# 토마토 (Silver 1)

### 문제

철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자 모양 상자의 칸에 하나씩 넣어서 창고에 보관한다.   

<p align="center">
  <img src="https://upload.acmicpc.net/de29c64f-dee7-4fe0-afa9-afd6fc4aad3a/-/preview/" width=215>
</p>

창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토의 인접한 곳은 왼쪽, 오른쪽, 앞, 뒤 네 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지, 그 최소 일수를 알고 싶어 한다.   

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.   

---

#### 입력

첫 줄에는 상자의 크기를 나타내는 두 정수 M,N이 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M,N ≤ 1,000 이다. 둘째 줄부터는 하나의 상자에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 상자에 담긴 토마토의 정보가 주어진다. 하나의 줄에는 상자 가로줄에 들어있는 토마토의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다.   

토마토가 하나 이상 있는 경우만 입력으로 주어진다.

---

#### 출력

여러분은 토마토가 모두 익을 때까지의 최소 날짜를 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.   

---

#### 예제 입력 1
~~~
6 4
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1 
~~~

#### 예제 출력 1
~~~
8
~~~

#### 예제 입력 2
~~~
6 4
0 -1 0 0 0 0
-1 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1
~~~

#### 예제 출력 2
~~~
-1
~~~

#### 예제 입력 3
~~~
6 4
1 -1 0 0 0 0
0 -1 0 0 0 0
0 0 0 0 -1 0
0 0 0 0 -1 1
~~~

#### 예제 출력 3
~~~
6
~~~

#### 예제 입력 4
~~~
5 5
-1 1 0 0 0
0 -1 -1 -1 0
0 -1 -1 -1 0
0 -1 -1 -1 0
0 0 0 0 0
~~~

#### 예제 출력 4
~~~
14
~~~

#### 예제 입력 5
~~~
2 2
1 -1
-1 1
~~~

#### 예제 출력 5
~~~
0
~~~

[출처](https://www.acmicpc.net/problem/7576)

---

### 문제풀이

이 문제는 bfs 문제입니다.   

이 문제를 연습을 위해서 java로 풀어봤습니다.   

일반적인 bfs문제인데, 문제를 풀면서 주의할 점을 아래와 같습니다.   

1. 익은 토마토의 개수가 1개 보다 많을 수 있으므로 익은 토마토를 모두 queue에 넣고 시작해야 합니다.   

2. 모든 토마토가 익지 않는 경우가 생기므로 예외처리 해주는 부분이 필요합니다.   

이 점들을 주의하면서 풀면 쉽게 풀 수 있는 문제입니다.   

그리고 이 문제를 풀면서 다시 한 번 느꼈는데, 알고리즘 문제 풀이는 python을 이용해 푸는 것보다   
java로 풀었을 때 코드 라인 수가 훨씬 많다는 것이 체감됐습니다.

---

#### 나의 풀이

~~~java
import java.util.*;

class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        String[] str = sc.nextLine().split(" ");
        int m = Integer.parseInt(str[0]);
        int n = Integer.parseInt(str[1]);
        
        int[][] tomatoes = new int[n][m];
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, 1, -1};
        
        Queue<Node> queue = new LinkedList<>();
        
        int answer = 0;
        
        for (int i = 0; i < n; i++) {
            str = sc.nextLine().split(" ");
            for (int j = 0; j < m; j++) {
                tomatoes[i][j] = Integer.parseInt(str[j]);
                
                if (tomatoes[i][j] == 1) {
                    queue.offer(new Node(i, j));
                }
            }
        }
        
        while (!queue.isEmpty()) {
            Node node = queue.poll();
            int x = node.x;
            int y = node.y;
            
            answer = Math.max(answer, tomatoes[x][y]);
            
            for (int i = 0; i < 4; i++) {
                int nx = dx[i] + x;
                int ny = dy[i] + y;
                
                if (0 <= nx && nx < n && 0 <= ny && ny < m
                        && tomatoes[nx][ny] == 0) {
                    queue.offer(new Node(nx, ny));
                    tomatoes[nx][ny] = tomatoes[x][y] + 1;
                }
            }
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (tomatoes[i][j] == 0) {
                    System.out.println(-1);
                    return;
                }
            }
        }
        
        System.out.println(answer-1);
    }
}

class Node {
    int x;
    int y;
    
    Node(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
~~~

---

#### 다른 사람의 풀이

~~~java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
    private static int M, N, cnt = 0, res = 0;
    private static int arr[][], dx[] = {-1, 1, 0, 0}, dy[] = {0, 0, -1, 1};
    private static Queue <int[]> q = new LinkedList<>();
    
    public static void bfs() {
        while(!q.isEmpty()) {
            for(int i = 0 ; i < 4; i++) {
                int x = q.peek()[0] + dx[i], y = q.peek()[1] + dy[i];
                if(x < 0 || x >= N || y < 0 || y >= M || arr[x][y] != 0) continue;
                q.offer(new int[] {x, y, q.peek()[2] + 1});
                arr[x][y] = 1;
                cnt--;
            }
            res = q.poll()[2];
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        M = sc.nextInt(); N = sc.nextInt();
        arr = new int[N][M];
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < M; j++) {
                arr[i][j] = sc.nextInt();
                if(arr[i][j] == 1) q.offer(new int[] {i, j, 0});
                else if(arr[i][j] == 0) cnt++; // 익지 않은 토마토 개수 세기
            }
        }
        bfs();
        if(cnt != 0) res = -1;
        System.out.println(res);
    }
}
~~~
