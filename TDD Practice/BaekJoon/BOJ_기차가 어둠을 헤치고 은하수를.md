# 기차가 어둠을 헤치고 은하수를 (Silver 2)

### 문제 설명

N개의 기차가 어둠을 헤치고 은하수를 건너려고 한다.

기차는 20개의 일렬로 된 좌석이 있고, 한 개의 좌석에는 한 명의 사람이 탈 수 있다. 

기차의 번호를 1번부터 N번으로 매길 때, 어떠한 기차에 대하여 M개의 명령이 주어진다.

명령의 종류는 4가지로 다음과 같다.

* 1 i x : i번째 기차에(1 ≤ i ≤ N) x번째 좌석에(1 ≤ x ≤ 20) 사람을 태워라. 이미 사람이 타있다면 , 아무런 행동을 하지 않는다.
* 2 i x : i번째 기차에 x번째 좌석에 앉은 사람은 하차한다. 만약 아무도 그자리에 앉아있지 않았다면, 아무런 행동을 하지 않는다.
* 3 i : i번째 기차에 앉아있는 승객들이 모두 한칸씩 뒤로간다. k번째 앉은 사람은 k+1번째로 이동하여 앉는다. 만약 20번째 자리에 사람이 앉아있었다면 그 사람은 이 명령 후에 하차한다.
* 4 i : i번째 기차에 앉아있는 승객들이 모두 한칸씩 앞으로간다. k번째 앉은 사람은 k-1 번째 자리로 이동하여 앉는다. 만약 1번째 자리에 사람이 앉아있었다면 그 사람은 이 명령 후에 하차한다.

M번의 명령 후에 1번째 기차부터 순서대로 한 기차씩 은하수를 건너는데 조건이 있다.

기차는 순서대로 지나가며 기차가 지나갈 때 승객이 앉은 상태를 목록에 기록하며 이미 목록에 존재하는 기록이라면 해당 기차는 은하수를 건널 수 없다.

예를 들면, 다음 그림을 예로 들었을 때, 1번째 기차와 같이 승객이 앉은 상태는 기록되지 않았기 때문에 은하수를 건널 수있다. 2번째 기차와 같은 상태도 기록되지 않았기 때문에 2번째 기차도 은하수를 건널 수 있다. 3번째 기차는 1번째 기차와 승객이 앉은 상태가 같으므로 은하수를 건널 수 없다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15787/1.png" width="392px">

처음에 주어지는 기차에는 아무도 사람이 타지 않는다.

은하수를 건널 수 있는 기차의 수를 출력하시오.

출처 : https://www.acmicpc.net/problem/15787

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int[] trains = new int[n];

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            List<Integer> command = new ArrayList<>();
            while (st.hasMoreTokens()) {
                command.add(Integer.parseInt(st.nextToken()));
            }

            int trainIndex = command.get(1) - 1;

            switch (command.get(0)) {
                case 1:
                    trains[trainIndex] = getOnTrain(trains[trainIndex], command.get(2));
                    break;
                case 2:
                    trains[trainIndex] = getOffTrain(trains[trainIndex], command.get(2));
                    break;
                case 3:
                    trains[trainIndex] = shiftBack(trains[trainIndex]);
                    break;
                case 4:
                    trains[trainIndex] = shiftForward(trains[trainIndex]);
                    break;
            }
        }
        System.out.println(Arrays.stream(trains).boxed().collect(Collectors.toSet()).size());
    }

    static int getOnTrain(int train, int x) {
        return train | (1 << --x);
    }

    static int getOffTrain(int train, int x) {
        return train & ~(1 << --x);
    }

    static int shiftBack(int train) {
        return (train << 1) % 1048576;
    }

    static int shiftForward(int train) {
        return train >> 1;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getOnTrainTest() {
        assertEquals(8, getOnTrain(0, 4));
        assertEquals(16, getOnTrain(0, 5));
        assertEquals(15, getOnTrain(15, 4));
        assertEquals(1, getOnTrain(0, 1));
        assertEquals(1, getOnTrain(1, 1));
    }

    @Test
    void getOffTrainTest() {
        assertEquals(7, getOffTrain(15, 4));
        assertEquals(14, getOffTrain(15, 1));
        assertEquals(0, getOffTrain(1, 1));
        assertEquals(0, getOffTrain(0, 1));
    }

    @Test
    void shiftBackTest() {
        assertEquals(30, shiftBack(15));
        assertEquals(0, shiftBack(0));
        assertEquals(2, shiftBack(1));
        assertEquals(1048574, shiftBack(1048575));
        assertEquals(0, shiftBack(524288));
    }

    @Test
    void shiftForwardTest() {
        assertEquals(7, shiftForward(15));
        assertEquals(0, shiftForward(1));
        assertEquals(0, shiftForward(0));
        assertEquals(8, shiftForward(16));
    }

    private int getOnTrain(int train, int x) {
        return train | (1 << --x);
    }

    private int getOffTrain(int train, int x) {
        return train & ~(1 << --x);
    }

    private int shiftBack(int train) {
        return (train << 1) % 1048576;
    }

    private int shiftForward(int train) {
        return train >> 1;
    }
}
~~~
