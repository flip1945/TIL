# 필터 (Silver 4)

### 문제 설명

숫자 9개가 오름차순이나 내림차순으로 정렬되어 있을 때, 중앙값은 다섯 번째 숫자이다. 예를 들어, 1, 3, 4, 1, 2, 6, 8, 4, 10의 중앙값은 4이다. (1 ≤ 1 ≤ 2 ≤ 3 ≤ 4 ≤ 4 ≤ 6 ≤ 8 ≤ 10)

이미지 I는 크기가 R × C인 2차원 픽셀이다. (3 ≤ R ≤ 40, 3 ≤ C ≤ 40) 각 픽셀은 어두운 정도 V를 나타낸다. (0 ≤ V ≤ 255)

중앙 필터는 이미지에 있는 노이즈를 제거하는 필터이다. 필터의 크기는 3 × 3이고, 이미지의 중앙값을 찾으면서 잡음을 제거한다.

예를 들어, 아래와 같은 6 × 5 이미지가 있다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/filter1.gif">

필터링된 이미지의 크기는 4 × 3이고, 아래와 같다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/filter2.gif">

가장 왼쪽 윗 행에 필터를 두고, 오른쪽으로 움직이면서 중앙값을 찾는다. 한 행을 모두 이동했으면, 다음 행으로 이동해 다시 중앙값을 찾는다. 아래와 같은 순서를 가진다.


<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/filter3.gif">

위의 그림에서 각각의 중앙값은 36, 36, 21이 된다. 이 값은 필터링된 이미지 J의 첫 행과 같다. 

이미지 I가 주어졌을 때, 필터링 된 이미지 J를 구하고, 값이 T보다 크거나 같은 픽셀의 수를 구하는 프로그램을 작성하시오.

예를 들어, T = 40일 때, 위의 예에서 정답은 7이다. 

출처 : https://www.acmicpc.net/problem/1895

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int row = Integer.parseInt(st.nextToken());
        int col = Integer.parseInt(st.nextToken());
        int[][] pixels = new int[row][col];
        for (int i = 0; i < row; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < col; j++) {
                pixels[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        int t = Integer.parseInt(br.readLine());

        System.out.println(filter(row, col, t, pixels));
    }

    static int filter(int row, int column, int t, int[][] pixels) {
        List<Integer> filteringPixels = new ArrayList<>();
        for (int i = 0; i < row - 2; i++) {
            for (int j = 0; j < column - 2; j++) {
                List<Integer> selected3x3Images = select3x3Image(pixels, i, j);
                filteringPixels.add(filterCenterImageValue(selected3x3Images));
            }
        }
        return getFilteredCountOverT(t, filteringPixels);
    }

    static List<Integer> select3x3Image(int[][] pixels, int startRow, int startColumn) {
        List<Integer> selectedPixels = new ArrayList<>();
        for (int i = startRow; i < startRow + 3; i++) {
            for (int j = startColumn; j < startColumn + 3; j++) {
                selectedPixels.add(pixels[i][j]);
            }
        }
        return selectedPixels;
    }

    static int filterCenterImageValue(List<Integer> pixels) {
        return pixels.stream()
                .sorted()
                .skip(4)
                .findFirst()
                .orElse(0);
    }

    static int getFilteredCountOverT(int t, List<Integer> filteringPixels) {
        return (int) filteringPixels.stream()
                .filter(i -> i >= t)
                .count();
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void filterCenterImageTest() {
        assertEquals(5, filterCenterImageValue(List.of(1, 2, 3, 4, 5, 6, 7, 8, 9)));
        assertEquals(36, filterCenterImageValue(List.of(49, 36, 73, 27, 88, 14, 99, 18, 36)));
        assertEquals(36, filterCenterImageValue(List.of(36, 73, 62, 88, 14, 11, 18, 36, 91)));
        assertEquals(21, filterCenterImageValue(List.of(73, 62, 21, 14, 11, 12, 36, 91, 21)));
    }

    @Test
    void filterTest() {
        assertEquals(7, filter(6, 5, 40, new int[][]{{49, 36, 73, 62, 21}, {27, 88, 14, 11, 12}, {99, 18, 36, 91, 21}, {45, 96, 72, 12, 10}, {12, 48, 49, 75, 56}, {12, 15, 48, 86, 78}}));
        assertEquals(1, filter(3, 3, 0, new int[][]{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}));
        assertEquals(0, filter(3, 3, 8, new int[][]{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}));
    }

    private int filter(int row, int column, int t, int[][] pixels) {
        List<Integer> filteringPixels = new ArrayList<>();
        for (int i = 0; i < row - 2; i++) {
            for (int j = 0; j < column - 2; j++) {
                List<Integer> selected3x3Images = select3x3Image(pixels, i, j);
                filteringPixels.add(filterCenterImageValue(selected3x3Images));
            }
        }
        return getFilteredCountOverT(t, filteringPixels);
    }

    private List<Integer> select3x3Image(int[][] pixels, int startRow, int startColumn) {
        List<Integer> selectedPixels = new ArrayList<>();
        for (int i = startRow; i < startRow + 3; i++) {
            for (int j = startColumn; j < startColumn + 3; j++) {
                selectedPixels.add(pixels[i][j]);
            }
        }
        return selectedPixels;
    }

    private int filterCenterImageValue(List<Integer> pixels) {
        return pixels.stream()
                .sorted()
                .skip(4)
                .findFirst()
                .orElse(0);
    }

    private int getFilteredCountOverT(int t, List<Integer> filteringPixels) {
        return (int) filteringPixels.stream()
                .filter(i -> i >= t)
                .count();
    }
}
~~~
