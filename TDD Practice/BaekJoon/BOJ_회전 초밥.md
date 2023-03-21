# 회전 초밥 (Gold 4)

### 문제 설명

회전 초밥 음식점에는 회전하는 벨트 위에 여러 가지 종류의 초밥이 접시에 담겨 놓여 있고, 손님은 이 중에서 자기가 좋아하는 초밥을 골라서 먹는다. 초밥의 종류를 번호로 표현할 때, 다음 그림은 회전 초밥 음식점의 벨트 상태의 예를 보여주고 있다. 벨트 위에는 같은 종류의 초밥이 둘 이상 있을 수 있다. 

<img src="https://upload.acmicpc.net/f29f0bd9-6114-4543-aa72-797208dc9cdd/-/preview/" width=300>

새로 문을 연 회전 초밥 음식점이 불경기로 영업이 어려워서, 다음과 같이 두 가지 행사를 통해서 매상을 올리고자 한다.

1. 원래 회전 초밥은 손님이 마음대로 초밥을  고르고, 먹은 초밥만큼 식대를 계산하지만, 벨트의 임의의 한 위치부터 k개의 접시를 연속해서 먹을 경우 할인된 정액 가격으로 제공한다. 
2. 각 고객에게 초밥의 종류 하나가 쓰인 쿠폰을 발행하고, 1번 행사에 참가할 경우 이 쿠폰에 적혀진 종류의 초밥 하나를 추가로 무료로 제공한다. 만약 이 번호에 적혀진 초밥이 현재 벨트 위에 없을 경우, 요리사가 새로 만들어 손님에게 제공한다.  

위 할인 행사에 참여하여 가능한 한 다양한 종류의 초밥을 먹으려고 한다. 위 그림의 예를 가지고 생각해보자. k=4이고, 30번 초밥을 쿠폰으로 받았다고 가정하자. 쿠폰을 고려하지 않으면 4가지 다른 초밥을 먹을 수 있는 경우는 (9, 7, 30, 2), (30, 2, 7, 9), (2, 7, 9, 25) 세 가지 경우가 있는데, 30번 초밥을 추가로 쿠폰으로 먹을 수 있으므로 (2, 7, 9, 25)를 고르면 5가지 종류의 초밥을 먹을 수 있다. 

회전 초밥 음식점의 벨트 상태, 메뉴에 있는 초밥의 가짓수, 연속해서 먹는 접시의 개수, 쿠폰 번호가 주어졌을 때, 손님이 먹을 수 있는 초밥 가짓수의 최댓값을 구하는 프로그램을 작성하시오. 

출처 : https://www.acmicpc.net/problem/15961

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int d = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());
        int c = Integer.parseInt(st.nextToken());
        int[] sushi = new int[n];
        for (int i = 0; i < n; i++) {
            sushi[i] = Integer.parseInt(br.readLine());
        }

        System.out.println(getMaxCountOfSushiType(n, d, sushi, k, c));
    }

    static int getMaxCountOfSushiType(int dishSize, int countOfSushiType, int[] sushi, int streak, int bonusSushi) {
        int maxCountOfSushiType = 0;
        int[] countOfSushiTypes = new int[countOfSushiType + 1];
        Set<Integer> subSushi = initSushi(sushi, countOfSushiTypes, streak);
        for (int i = 0; i < dishSize; i++) {
            maxCountOfSushiType = Math.max(maxCountOfSushiType, getCountOfSushiType(subSushi, bonusSushi));
            rotateNextSubSushi(subSushi, countOfSushiTypes, sushi[i], sushi[(i + streak) % dishSize]);
        }
        return maxCountOfSushiType;
    }

    static Set<Integer> initSushi(int[] sushi, int[] countOfSushiTypes, int streak) {
        Set<Integer> initSushi = new HashSet<>();
        for (int i = 0; i < streak; i++) {
            countOfSushiTypes[sushi[i]]++;
            initSushi.add(sushi[i]);
        }
        return initSushi;
    }

    static void rotateNextSubSushi(Set<Integer> sushi, int[] countOfSushiTypes, int currentSushi, int nextSushi) {
        countOfSushiTypes[currentSushi]--;
        if (countOfSushiTypes[currentSushi] == 0) {
            sushi.remove(currentSushi);
        }
        countOfSushiTypes[nextSushi]++;
        sushi.add(nextSushi);
    }

    static int getCountOfSushiType(Set<Integer> sushiTypes, int bonusSushi) {
        int countOfSushiType = sushiTypes.size();
        return !sushiTypes.contains(bonusSushi) ? countOfSushiType + 1 : countOfSushiType;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getCountOfSushiTypeTest() {
        assertEquals(1, getCountOfSushiType(Set.of(1), 1));
        assertEquals(2, getCountOfSushiType(Set.of(1, 2), 1));
        assertEquals(3, getCountOfSushiType(Set.of(1, 2, 3), 1));
        assertEquals(4, getCountOfSushiType(Set.of(1, 2, 3), 4));
        assertEquals(2, getCountOfSushiType(Set.of(1), 2));
    }

    @Test
    void getMaxCountOfSushiTypeTest() {
        assertEquals(1, getMaxCountOfSushiType(3, 2, new int[]{1, 1, 1}, 2, 1));
        assertEquals(3, getMaxCountOfSushiType(4, 4, new int[]{1, 2, 3, 4}, 2, 1));
        assertEquals(2, getMaxCountOfSushiType(4, 2, new int[]{2, 2, 2, 2}, 2, 1));
        assertEquals(5, getMaxCountOfSushiType(8, 30, new int[]{7, 9, 7, 30, 2, 7, 9, 25}, 4, 30));
        assertEquals(4, getMaxCountOfSushiType(8, 50, new int[]{2, 7, 9, 25, 7, 9, 7, 30}, 4, 7));
        assertEquals(5, getMaxCountOfSushiType(12, 10, new int[]{1, 2, 1, 2, 3, 4, 3, 4, 5, 6, 3, 4}, 4, 7));
    }

    private int getMaxCountOfSushiType(int dishSize, int countOfSushiType, int[] sushi, int streak, int bonusSushi) {
        int maxCountOfSushiType = 0;
        int[] countOfSushiTypes = new int[countOfSushiType + 1];
        Set<Integer> subSushi = initSushi(sushi, countOfSushiTypes, streak);
        for (int i = 0; i < dishSize; i++) {
            maxCountOfSushiType = Math.max(maxCountOfSushiType, getCountOfSushiType(subSushi, bonusSushi));
            rotateNextSubSushi(subSushi, countOfSushiTypes, sushi[i], sushi[(i + streak) % dishSize]);
        }
        return maxCountOfSushiType;
    }

    private Set<Integer> initSushi(int[] sushi, int[] countOfSushiTypes, int streak) {
        Set<Integer> initSushi = new HashSet<>();
        for (int i = 0; i < streak; i++) {
            countOfSushiTypes[sushi[i]]++;
            initSushi.add(sushi[i]);
        }
        return initSushi;
    }

    private void rotateNextSubSushi(Set<Integer> sushi, int[] countOfSushiTypes, int currentSushi, int nextSushi) {
        countOfSushiTypes[currentSushi]--;
        if (countOfSushiTypes[currentSushi] == 0) {
            sushi.remove(currentSushi);
        }
        countOfSushiTypes[nextSushi]++;
        sushi.add(nextSushi);
    }

    private int getCountOfSushiType(Set<Integer> sushiTypes, int bonusSushi) {
        int countOfSushiType = sushiTypes.size();
        return !sushiTypes.contains(bonusSushi) ? countOfSushiType + 1 : countOfSushiType;
    }
}

~~~
