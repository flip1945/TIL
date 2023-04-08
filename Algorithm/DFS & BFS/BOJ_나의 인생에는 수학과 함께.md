# 나의 인생에는 수학과 함께 (Gold 5)

### 문제

세현이의 인생의 목표는 1분 1초 모든 순간 수학과 함께 살아가는 것이다. 그렇기 때문에 매일 수학을 생각하면서 살아가고 있다. 세현이는 밥을 먹을 때도 쌀알의 수를 계산하여 칼로리를 바로 계산하고 한걸음 한걸음 보폭을 계산하여 자신의 활동량을 확인하며 인생의 목표를 실행하며 살아가고 있다.  그런 세현이는 매일 학교를 가면서 지나가는 길에도 수학을 적용시키고 싶었다.

세현이네 집에서 학교까지 가는 길은 N x N 크기의 바둑판과 같다. 그리고 각 블록은 1x1 정사각형으로 구분 지을 수 있다. 세현이는 그 블록마다 숫자와 연산자가 존재한다고 생각해서 임의의 숫자와 연산자를 각 블록에 넣어 바둑판을 만들었다.

세현이는 학교에서 집으로 가는 경로에서 만나는 숫자와 연산자의 연산 결과의 최댓값과 최솟값을 구하려고 한다. 세현이는 항상 자신의 집 (1, 1)에서 학교 (N, N)까지 최단거리로 이동한다. 최단거리로 이동하기 위해서는 오른쪽과 아래쪽으로만 이동해야 한다.

<img src="https://upload.acmicpc.net/52b1ed3b-b434-4cb7-b532-ce8658764c08/-/preview/" width=600>

위와 같이 N = 5 인 5 x 5 바둑판을 만들었다고 해보자.

그림 1의 경로를 통해 집(1, 1)에서 학교(N, N)까지 어떻게 연산이 되는지 확인해보자. 경로에서 만나는 연산자들의 우선순위는 고려하지 않는다.

 1. 5 → × → 4 = 20
 2. 20 → + → 5 = 25
 3. 25 → ×→ 5 = 125
 4. 125 → + → 2 = 127

그림 1은 최댓값을 가지는 경로이며 127이라는 최댓값을 얻을 수 있다.

그리고 위와 같이 연산하여 그림 2의 경로를 통해서 최솟값으로 3을 얻을 수 있다.

세현이는 이 길을 걸으면서 최댓값과 최솟값을 암산하다가 교통사고를 당해 현재 인하대학교 병원에 입원했다. 아픈 세현이를 위해 최댓값과 최솟값을 구해주자.

---

#### 입력

첫 번째 줄에는 지도의 크기 N이 주어진다. (3 ≤ N ≤ 5, N은 홀수) 

그 다음 N 줄에는 N개의 숫자 또는 연산자가 빈칸을 사이에 두고 주어지며, 세현이네 집 (1, 1)과 학교 (N, N)는 반드시 숫자로 주어진다.

그리고 숫자 다음에는 연산자, 연산자 다음에는 숫자가 나오도록 주어진다. 주어지는 숫자는 0이상 5이하의 정수, 연산자는 (‘+’, ‘-’, ‘*’) 만 주어진다.

---

#### 출력

연산 결과의 최댓값과 최솟값을 하나의 공백을 두고 출력한다. 연산자 우선순위는 고려하지 않는다.

출처 : https://www.acmicpc.net/problem/17265

---

### 문제풀이

이 문제는 dfs 문제입니다.

n의 크기가 3에서 5 사이로 작은 숫자기 때문에 문제에서 주어진대로 모든 경로에 대해 dfs를 실행하면됩니다.

이 문제를 풀면서 주의할 점은 현재까지 계산한 숫자를 어느 부분에서 정할 것인지와 최대값의 초기값을 0이 아니라 -로 정해야 한다는 점입니다.

---

#### 나의 풀이

~~~java
import java.util.*;

public class Main {
    static String[][] map;
    static int max = Integer.MIN_VALUE;
    static int min = Integer.MAX_VALUE;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        map = new String[n][n];
        for (int i = 0; i < n; i++) {
            map[i] = scanner.nextLine().split(" ");
        }

        dfs(n, 0, 0, 0, '+');

        System.out.println(max  + " " +  min);
    }

    static void dfs(int n, int sum, int row, int col, char sign) {
        char currentCharacter = map[row][col].charAt(0);
        if (Character.isDigit(currentCharacter)) {
            sum = getSum(sum, sign, currentCharacter);
        } else {
            sign = currentCharacter;
        }

        if (row == n - 1 && col == n - 1) {
            max = Math.max(max, sum);
            min = Math.min(min, sum);
            return;
        }

        int[] dx = new int[]{0, 1};
        int[] dy = new int[]{1, 0};

        for (int i = 0; i < 2; i++) {
            int nextRow = row + dx[i];
            int nextCol = col + dy[i];

            if (nextRow < n && nextCol < n) {
                dfs(n, sum, nextRow, nextCol, sign);
            }
        }
    }

    private static int getSum(int sum, char sign, char currentCharacter) {
        if (sign == '+') {
            return sum + Character.getNumericValue(currentCharacter);
        } else if (sign == '-') {
            return sum - Character.getNumericValue(currentCharacter);
        }
        return sum * Character.getNumericValue(currentCharacter);
    }
}
~~~
