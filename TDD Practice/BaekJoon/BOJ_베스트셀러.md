# 베스트셀러 (Silver 4)

### 문제 설명

김형택은 탑문고의 직원이다. 김형택은 계산대에서 계산을 하는 직원이다. 김형택은 그날 근무가 끝난 후에, 오늘 판매한 책의 제목을 보면서 가장 많이 팔린 책의 제목을 칠판에 써놓는 일도 같이 하고 있다.

오늘 하루 동안 팔린 책의 제목이 입력으로 들어왔을 때, 가장 많이 팔린 책의 제목을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1302

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String[] books = IntStream.range(0, n).mapToObj(i -> scanner.nextLine()).toArray(String[]::new);

        System.out.println(getDailyBestsellerName(books));
    }

    static String getDailyBestsellerName(String[] books) {
        Map<String, Integer> sellingCountOfBooks = calculateSellingCountOfBooks(books);
        return findBestSellerName(sellingCountOfBooks);
    }

    static Map<String, Integer> calculateSellingCountOfBooks(String[] books) {
        Map<String, Integer> sellingCountOfBooks = new TreeMap<>();
        for (String book : books) {
            int sellingCount = sellingCountOfBooks.getOrDefault(book, 0) + 1;
            sellingCountOfBooks.put(book, sellingCount);
        }
        return sellingCountOfBooks;
    }

    static String findBestSellerName(Map<String, Integer> countOfSellingBooks) {
        int maxSellingCountOfBooks = Collections.max(countOfSellingBooks.values());
        return countOfSellingBooks.entrySet().stream()
                .filter(e -> e.getValue().equals(maxSellingCountOfBooks))
                .map(Map.Entry::getKey)
                .findFirst()
                .orElse("");
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
    void getDailyBestsellerNameTest() {
        assertEquals("top", getDailyBestsellerName(new String[]{"top", "top", "kimtop"}));
        assertEquals("kimtop", getDailyBestsellerName(new String[]{"top", "kimtop", "kimtop"}));
        assertEquals("kimtop", getDailyBestsellerName(new String[]{"top", "kimtop"}));
        assertEquals("table", getDailyBestsellerName(new String[]{"table", "chair", "table", "table", "lamp", "door", "lamp", "table", "chair"}));
        assertEquals("a", getDailyBestsellerName(new String[]{"a", "a", "a", "b", "b", "b"}));
        assertEquals("chocolate", getDailyBestsellerName(new String[]{"icecream", "peanuts", "peanuts", "chocolate", "candy", "chocolate", "icecream", "apple"}));
        assertEquals("soul", getDailyBestsellerName(new String[]{"soul"}));
    }

    private String getDailyBestsellerName(String[] books) {
        Map<String, Integer> sellingCountOfBooks = calculateSellingCountOfBooks(books);
        return findBestSellerName(sellingCountOfBooks);
    }

    private Map<String, Integer> calculateSellingCountOfBooks(String[] books) {
        Map<String, Integer> sellingCountOfBooks = new TreeMap<>();
        for (String book : books) {
            int sellingCount = sellingCountOfBooks.getOrDefault(book, 0) + 1;
            sellingCountOfBooks.put(book, sellingCount);
        }
        return sellingCountOfBooks;
    }

    private String findBestSellerName(Map<String, Integer> countOfSellingBooks) {
        int maxSellingCountOfBooks = Collections.max(countOfSellingBooks.values());
        return countOfSellingBooks.entrySet().stream()
                .filter(e -> e.getValue().equals(maxSellingCountOfBooks))
                .map(Map.Entry::getKey)
                .findFirst()
                .orElse("");
    }
}
~~~
