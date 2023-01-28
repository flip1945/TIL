# 파닭파닭 (Silver 2)

### 문제

평소 요리에 관심이 많은 승균이는 치킨집을 개업하였다. 승균이네 치킨집은 파닭이 주메뉴다. 승균이는 가게를 오픈하기 전에 남부시장에 들러서 길이가 일정하지 않은 파를 여러 개 구매하였다. 승균이는 파닭의 일정한 맛을 유지하기 위해 각각의 파닭에 같은 양의 파를 넣는다. 또 파닭 맛은 파의 양에 따라 좌우된다고 생각하기 때문에 될 수 있는 한 파의 양을 최대한 많이 넣으려고 한다. (하지만 하나의 파닭에는 하나 이상의 파가 들어가면 안 된다.) 힘든 하루를 마치고 승균이는 파닭을 만들고 남은 파를 라면에 넣어 먹으려고 한다. 이때 라면에 넣을 파의 양을 구하는 프로그램을 작성하시오. 승균이네 치킨집 자는 정수만 표현되기 때문에 정수의 크기로만 자를 수 있다.

---

#### 입력

첫째 줄에 승균이가 시장에서 사 온 파의 개수 S(1 ≤ S ≤ 1,000,000), 그리고 주문받은 파닭의 수 C(1 ≤ C ≤ 1,000,000)가 입력된다. 파의 개수는 항상 파닭의 수를 넘지 않는다. (S ≤ C) 그 후, S 줄에 걸쳐 파의 길이 L(1 ≤ L ≤ 1,000,000,000)이 정수로 입력된다.

---

#### 출력

승균이가 라면에 넣을 파의 양을 출력한다.

출처 : https://www.acmicpc.net/problem/14627

---

### 문제풀이

이번 문제는 파라메트릭 서치 문제입니다.

이번에는 종료 조건과 증감 조건을 잘 설정했는데, 푸는 데 오래걸린 문제였습니다.

최대 파의 길이를 파라메트릭 서치로 구하고 마지막에 남의 파의 길이의 합을 구해야 합니다.

그런데 남은 파의 길이를 구할 때 모든 파의 길이의 합을 구해야 하는데, 모든 파의 길이의 합이 int의 범위를 넘어갈 수 있기 때문에 long을 사용해야 한다는 점을 주의해야 합니다.

매번 파이썬으로 알고리즘 문제를 풀다보니 타입에 대한 부분을 신경쓰지 않았는데, 앞으로는 주의해야겠습니다.

---

#### 나의 풀이

~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine());
        int s = Integer.parseInt(st.nextToken());
        int c = Integer.parseInt(st.nextToken());

        List<Integer> greenOnions = new ArrayList<>();
        for (int i = 0; i < s; i++) {
            greenOnions.add(Integer.parseInt(bf.readLine()));
        }

        int start = 0;
        int end = 1000000001;

        while (start <= end) {
            int lengthOfGreenOnion = (end - start) / 2 + start;

            int countOfGreenOnion = getCountOfGreenOnion(greenOnions, lengthOfGreenOnion);

            if (countOfGreenOnion >= c) {
                start = lengthOfGreenOnion + 1;
            } else {
                end = lengthOfGreenOnion - 1;
            }
        }

        System.out.println(getAnswer(greenOnions, end, c));
    }

    private static int getCountOfGreenOnion(List<Integer> greenOnions, int lengthOfGreenOnion) {
        if (lengthOfGreenOnion == 0) {
            return 1000000001;
        }

        int countOfGreenOnion = 0;
        for (Integer greenOnion : greenOnions) {
            countOfGreenOnion += greenOnion / lengthOfGreenOnion;
        }
        return countOfGreenOnion;
    }

    private static long getAnswer(List<Integer> greenOnions, int lengthOfGreenOnion, int countOfChicken) {
        return greenOnions.stream().mapToLong(Integer::longValue).sum() - ((long) lengthOfGreenOnion * countOfChicken);
    }
}
~~~
