# 페이지 세기 (Silver 5)

### 문제 설명

워드, 한글, 메모장과 같은 워드 프로세서에서 인쇄를 할 때, 페이지 범위를 직접 입력하여 지정할 수 있다. 예를 들면, 다음과 같이 입력할 수 있다.

10-15,25-28,8-4,13-20,9,8-8

사용자는 위처럼 인쇄하고자 하는 범위를 콤마로 구분하여 입력할 수 있다.

각 인쇄 범위는 양의 정수 하나 또는 하이픈(-)로 구분된 두 양의 정수이다. 수 두 개로 이루어진 범위에서 앞의 수를 low, 뒤의 수를 high라고 한다. 만약, low > high인 경우에는 이 범위는 인쇄하지 않는다. 또, 인쇄 범위가 문서의 범위를 넘어가는 경우에는 출력할 수 있는 페이지만 출력한다. 페이지 번호는 1부터 시작한다.

인쇄 범위는 겹칠 수 있다. 겹치는 페이지는 여러 번 인쇄하는 것이 아니고, 한 번만 인쇄해야 한다. (위의 예제에서 13, 14, 15는 두 범위에 포함된다)

인쇄 범위가 주어졌을 때, 출력해야 하는 페이지의 수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/4821

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n;
        while (true) {
            n = Integer.parseInt(br.readLine());
            if (n == 0) {
                break;
            }
            Printer printer = new Printer(n);
            List<String> ranges = inputRages(br.readLine());
            System.out.println(printer.getRequiredPageCount(ranges));
        }
    }

    private static List<String> inputRages(String ranges) {
        return Arrays.stream(ranges.split(","))
                .collect(Collectors.toList());
    }
}

class Printer {
    private final int totalPage;
    private final int[] pages;

    public Printer(int totalPage) {
        this.totalPage = totalPage;
        this.pages = new int[totalPage + 1];
    }

    public int getRequiredPageCount(List<String> ranges) {
        for (String range : ranges) {
            PageRange pageRange = toPageRange(range);
            pageRange.applyPageRange(pages);
        }
        return calculateRequiredPageCount();
    }

    private PageRange toPageRange(String range) {
        if (range.contains("-")) {
            return new MultiPageRange(totalPage, range);
        }
        return new SinglePageRange(totalPage, range);
    }

    private int calculateRequiredPageCount() {
        return (int) Arrays.stream(pages)
                .filter(page -> page == 1)
                .count();
    }
}

interface PageRange {
    void applyPageRange(int[] pages);
}

class SinglePageRange implements PageRange {
    private final int totalPage;
    private final int page;

    public SinglePageRange(int totalPage, String range) {
        this.totalPage = totalPage;
        this.page = Integer.parseInt(range);
    }

    @Override
    public void applyPageRange(int[] pages) {
        if (page <= totalPage) {
            pages[page] = 1;
        }
    }
}

class MultiPageRange implements PageRange {
    private final int totalPage;
    private final int start;
    private final int end;

    public MultiPageRange(int totalPage, String range) {
        this.totalPage = totalPage;
        this.start = Math.max(Integer.parseInt(range.split("-")[0]), 0);
        this.end = Math.min(Integer.parseInt(range.split("-")[1]), totalPage);
    }

    @Override
    public void applyPageRange(int[] pages) {
        if (start <= totalPage) {
            applyMultiPageRange(pages);
        }
    }

    private void applyMultiPageRange(int[] pages) {
        for (int i = start; i <= end; i++) {
            pages[i] = 1;
        }
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePageRanges")
    @DisplayName("인쇄 범위가 주어졌을 때 출력해야 하는 페이지의 수를 반환한다.")
    void getRequiredPageCountTest(int totalPage, List<String> ranges, int expected) {
        Printer sut = new Printer(totalPage);

        int actual = sut.getRequiredPageCount(ranges);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePageRanges() {
        return Stream.of(
                Arguments.of(5, List.of("1-1"), 1),
                Arguments.of(5, List.of("1-2"), 2),
                Arguments.of(5, List.of("1-5"), 5),
                Arguments.of(5, List.of("1-3", "2-4"), 4),
                Arguments.of(5, List.of("1-1", "4-5"), 3),
                Arguments.of(5, List.of("2-1", "4-5"), 2),
                Arguments.of(5, List.of("4-5", "8-10"), 2),
                Arguments.of(5, List.of("1-15"), 5),
                Arguments.of(5, List.of("15"), 0),
                Arguments.of(30, List.of("10-15", "25-28", "8-4", "13-20", "9", "8-8"), 17),
                Arguments.of(19, List.of("10-15", "25-28", "8-4", "13-20", "9", "8-8"), 12)
        );
    }
}

class Printer {
    private final int totalPage;
    private final int[] pages;

    public Printer(int totalPage) {
        this.totalPage = totalPage;
        this.pages = new int[totalPage + 1];
    }

    public int getRequiredPageCount(List<String> ranges) {
        for (String range : ranges) {
            PageRange pageRange = toPageRange(range);
            pageRange.applyPageRange(pages);
        }
        return calculateRequiredPageCount();
    }

    private PageRange toPageRange(String range) {
        if (range.contains("-")) {
            return new MultiPageRange(totalPage, range);
        }
        return new SinglePageRange(totalPage, range);
    }

    private int calculateRequiredPageCount() {
        return (int) Arrays.stream(pages)
                .filter(page -> page == 1)
                .count();
    }
}

interface PageRange {
    void applyPageRange(int[] pages);
}

class SinglePageRange implements PageRange {
    private final int totalPage;
    private final int page;

    public SinglePageRange(int totalPage, String range) {
        this.totalPage = totalPage;
        this.page = Integer.parseInt(range);
    }

    @Override
    public void applyPageRange(int[] pages) {
        if (page <= totalPage) {
            pages[page] = 1;
        }
    }
}

class MultiPageRange implements PageRange {
    private final int totalPage;
    private final int start;
    private final int end;

    public MultiPageRange(int totalPage, String range) {
        this.totalPage = totalPage;
        this.start = Math.max(Integer.parseInt(range.split("-")[0]), 0);
        this.end = Math.min(Integer.parseInt(range.split("-")[1]), totalPage);
    }

    @Override
    public void applyPageRange(int[] pages) {
        if (start <= totalPage) {
            applyMultiPageRange(pages);
        }
    }

    private void applyMultiPageRange(int[] pages) {
        for (int i = start; i <= end; i++) {
            pages[i] = 1;
        }
    }
}
~~~
