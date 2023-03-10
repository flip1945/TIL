# 문서 검색 (Silver 4)

### 문제 설명

세준이는 영어로만 이루어진 어떤 문서를 검색하는 함수를 만들려고 한다. 이 함수는 어떤 단어가 총 몇 번 등장하는지 세려고 한다. 그러나, 세준이의 함수는 중복되어 세는 것은 빼고 세야 한다. 예를 들어, 문서가 abababa이고, 그리고 찾으려는 단어가 ababa라면, 세준이의 이 함수는 이 단어를 0번부터 찾을 수 있고, 2번부터도 찾을 수 있다. 그러나 동시에 셀 수는 없다.

세준이는 문서와 검색하려는 단어가 주어졌을 때, 그 단어가 최대 몇 번 중복되지 않게 등장하는지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1543

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String document = scanner.nextLine();
        String word = scanner.nextLine();

        System.out.println(getCountOfWordInDocument(document, word));
    }

    static int getCountOfWordInDocument(String document, String word) {
        int countOfWordInDocument = 0;
        int index = 0;
        int wordSize = word.length();
        while (index <= document.length() - wordSize) {
            boolean contains = containsWordInDocument(document.substring(index, index + wordSize), word);
            countOfWordInDocument = addCountIfContains(countOfWordInDocument, contains);
            index = getNextIndex(index, contains, wordSize);
        }
        return countOfWordInDocument;
    }

    static boolean containsWordInDocument(String document, String word) {
        return word.equals(document);
    }

    static int addCountIfContains(int countOfWordInDocument, boolean contains) {
        return (contains) ? countOfWordInDocument + 1 : countOfWordInDocument;
    }

    static int getNextIndex(int index, boolean contains, int wordSize) {
        return (contains) ? index + wordSize : index + 1;
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
    void getCountOfWordInDocumentTest() {
        assertEquals(1, getCountOfWordInDocument("a", "a"));
        assertEquals(0, getCountOfWordInDocument("a", "b"));
        assertEquals(2, getCountOfWordInDocument("aa", "a"));
        assertEquals(1, getCountOfWordInDocument("aa", "aa"));
        assertEquals(1, getCountOfWordInDocument("aaa", "aa"));
        assertEquals(2, getCountOfWordInDocument("ababababa", "aba"));
        assertEquals(2, getCountOfWordInDocument("a a a a a", "a a"));
        assertEquals(1, getCountOfWordInDocument("ababababa", "ababa"));
        assertEquals(3, getCountOfWordInDocument("aaaaaaa", "aa"));
    }

    private int getCountOfWordInDocument(String document, String word) {
        int countOfWordInDocument = 0;
        int index = 0;
        int wordSize = word.length();
        while (index <= document.length() - wordSize) {
            boolean contains = containsWordInDocument(document.substring(index, index + wordSize), word);
            countOfWordInDocument = addCountIfContains(countOfWordInDocument, contains);
            index = getNextIndex(index, contains, wordSize);
        }
        return countOfWordInDocument;
    }

    private boolean containsWordInDocument(String document, String word) {
        return word.equals(document);
    }

    private int addCountIfContains(int countOfWordInDocument, boolean contains) {
        return (contains) ? countOfWordInDocument + 1 : countOfWordInDocument;
    }

    private int getNextIndex(int index, boolean contains, int wordSize) {
        return (contains) ? index + wordSize : index + 1;
    }
}
~~~
