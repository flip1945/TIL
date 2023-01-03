# 동방 프로젝트 (Small) (Silver 5)

### 문제 설명

동아리방이 가지고 싶었던 병찬이는 LINK 사업단에 문의하여 N개의 방 중의 하나를 얻을 기회를 얻었다. 일자로 되어있는 건물에 N개의 방은 일직선상에 존재하며, 각 방에는 번호가 매겨져 있다. 맨 왼쪽 방의 번호는 1번이며, 순서대로 증가하여 맨 오른쪽 방의 번호가 N번이다. 각 방 사이에는 방을 구분하는 벽이 존재한다.

물론 병찬이 외에도 많은 사람이 동아리방을 원한다. 다행히 방은 충분했기에 병찬이는 안심하고 있었지만…

그때였다.

빅-종빈빌런이 나타나 건물 벽을 허물기 시작한 것이다! 빅-종빈빌런은 다음과 같은 규칙으로 벽을 무너뜨린다.

* x < y 를 만족하는 두 방에 대해서 x번 방부터 y번 방 사이에 있는 모든 벽을 허문다.
* 두 방 사이의 벽이 허물어지면 두 방은 하나의 방으로 합쳐진다.
* 이미 허물어진 벽이 존재한다면 무시하고 다음 벽을 허문다.
* 빅-종빈빌런은 건물이 무너지는 걸 원치 않기 때문에, 1번 방의 왼쪽 벽과 N번 방의 오른쪽 벽(즉, 바깥과 연결된 벽)은 허물지 않는다.

동아리 방의 개수가 점점 줄어들자 병찬이는 초조해졌다. 이에 병찬이는 동아리방을 얻을 수 있는지에 대한 확률을 계산하기 위해 남는 동아리방의 수를 구하고 싶어 한다. 병찬이를 위해 빅-종빈빌런의 행동 횟수 M과 동방의 개수 N이 주어졌을 때, 남은 동아리방의 수를 구해주자.

출처 : https://www.acmicpc.net/problem/14594

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        int m = Integer.parseInt(scanner.nextLine());
        int[] wall = new int[n - 1];
        Arrays.fill(wall, 1);

        for (int i = 0; i < m; i++) {
            StringTokenizer st = new StringTokenizer(scanner.nextLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            deleteWall(wall, start, end);
        }

        System.out.println(Arrays.stream(wall).sum() + 1);
    }

    private static void deleteWall(int[] wall, int start, int end) {
        for (int i = start - 1; i < end - 1; i++) {
            wall[i] = 0;
        }
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;

class MainTest {

    @Test
    void deleteWallTest() {
        int[] wall1 = new int[]{1, 1, 1, 1};
        int[] wall2 = new int[]{1, 1, 1, 1};
        int[] wall3 = new int[]{1, 1, 1, 1};
        int start1 = 1;
        int start2 = 2;
        int end1 = 2;
        int end2 = 4;
        int end3 = 5;

        assertArrayEquals(new int[]{0, 1, 1, 1}, deleteWall(wall1, start1, end1));
        assertArrayEquals(new int[]{1, 0, 0, 1}, deleteWall(wall2, start2, end2));
        assertArrayEquals(new int[]{0, 0, 0, 0}, deleteWall(wall3, start1, end3));
    }

    private int[] deleteWall(int[] wall, int start, int end) {
        for (int i = start - 1; i < end - 1; i++) {
            wall[i] = 0;
        }
        return wall;
    }
}
~~~
