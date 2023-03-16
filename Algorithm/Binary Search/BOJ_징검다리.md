# 징검다리 (Silver 3)

### 문제

승택이는 강을 건너려 한다.

승택이는 수영을 못하기 때문에, 강에 놓인 징검다리를 밟고 건너갈 것이다.

승택이는 수영은 못하지만 제자리뛰기는 정말 잘한다. 원하는 어느 곳으로든지 점프해서 바로 갈 수가 있다.

승택이는 이제 강의 한쪽 변 앞에 서 있다.

강엔 1번부터 시작해 2번, 3번, ... , N번 징검다리가 차례대로 놓여 있다.

강의 폭이 넓은 탓에 징검다리의 수는 엄청나게 많다.

이 징검다리를 모두 밟고 싶지는 않았던 승택이는 제자리뛰기 실력을 발휘해 적절한 개수의 징검다리만을 밟고 가기로 했다.

물론 강 건너편으로 바로 점프하는 것도 가능하지만, 더 재미있게 강을 건너기 위해 승택이는 다음과 같은 규칙을 정했다.

1. 첫 징검다리는 점프해서 아무 것이나 밟을 수 있다. 이 점프가 첫 점프이다.
2. 두 번째 점프부터는 이전에 점프한 거리보다 1 이상 더 긴 거리를 뛰어야만 한다.
3. N번 징검다리는 반드시 밟아야 한다.
4. N번 징검다리를 밟은 후 강 건너로 이동할 땐 점프를 하지 않으므로 위의 규칙이 적용되지 않는다.

승택이가 위의 규칙을 지키며 강을 건널 때, 밟을 수 있는 징검다리의 최대 수는 몇 개일까?

---

#### 입력

첫째 줄에 테스트 케이스의 수 T가 주어진다.

각 테스트 케이스는 정수 한 개로 이루어져 있으며, 징검다리의 총 수 N을 의미한다. (1 ≤ N ≤ 10^16)

---

#### 출력

각 테스트 케이스마다 한 줄에 승택이가 밟을 수 있는 최대 징검다리 수를 출력한다.

출처 : https://www.acmicpc.net/problem/11561

---

### 문제풀이

이번 문제는 이분 탐색 문제입니다.

문제 자체는 어려운 문제는 아닌데 탐색 조건을 구할 때, 가우스의 합 공식을 이용해 탐색 조건을 구해야 해서 문제가 있습니다.

처음에는 end를 n으로 주고 풀었는데 그렇게 돼면 long의 범위를 넘어가기 때문에 end의 값을 특정값 이하로 제한해야 합니다.

end의 초기값을 2억 정도로 맞추면 문제를 통과할 수 있습니다.

---

#### 나의 풀이

~~~java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(br.readLine());
        while (t --> 0) {
            System.out.println(binarySearch(Long.parseLong(br.readLine())));
        }
    }

    static long binarySearch(long n) {
        long start = 0;
        long end = 200000000;
        while (start <= end) {
            long mid = (end - start) / 2 + start;
            long sum = mid * (mid + 1) / 2;

            if (sum == n) {
                return mid;
            } else if (sum < n) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        return end;
    }
}
~~~
