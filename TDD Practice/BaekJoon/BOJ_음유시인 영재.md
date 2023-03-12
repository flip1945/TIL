# 음유시인 영재 (Silver 3)

### 문제 설명

감수성이 뛰어난 음유시인 영재는 일상생활 중에 번뜩 시상이 떠오르곤 한다.

하지만 기억력이 좋지 못한 영재는 시상이 떠오르면 그 순간 컴퓨터로 기록해야만 안 까먹는다! 시는 대문자, 소문자 알파벳과 빈칸으로 이루어져 있다. 시상은 매번 훌륭하지만 제목 짓는 센스가 부족한 영재는 시에 나오는 단어들의 첫 글자를 대문자로 바꾼 뒤 순서대로 이어서 제목으로 만든다. 만약 시의 내용이 'There is no cow level' 이라면 시의 제목은 'TINCL'이 된다.

시도 때도 없이 시를 기록하느라 낡아버린 영재의 키보드는 수명이 얼마 남지 않았다. 앞으로 스페이스 바와 영자판을 누를 수 있는 횟수가 정해져 있어 이를 초과하면 키보드가 수명이 다 하여 어떠한 작업도 하지 못하게 된다. 하나 다행인 점은, 키보드를 쓸 때 같은 문자가 연속으로 나오거나 빈칸이 연속으로 나오는 경우 영재는 자판을 꾹 눌러 한 번만 사용해서 키보드를 좀 더 효율적으로 쓸 수 있다. (A와 a는 다른 문자이므로 'Aa'는 2번의 a자판을 누른 것으로 한다.)

시의 내용과 시의 제목은 Enter 키로 구분된다. 다행히 Shift 키와 Enter 키는 항상 수명이 무한한 상황이다!

음유시인 영재가 이번에 지은 시의 내용과 스페이스 바와 영자판을 누를 수 있는 횟수가 주어졌을 때, 시의 내용과 제목을 모두 기록할 수 있다면 시의 제목을 출력하고, 만약 키보드의 수명이 다 하여 기록을 완벽하게 못 하게 된다면 -1을 출력하여라.

출처 : https://www.acmicpc.net/problem/19948

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String poem = scanner.nextLine();
        int remainSpaceBarCount = scanner.nextInt();
        int[] remainLettersCounts = new int[26];
        for (int i = 0; i < 26; i++) {
            remainLettersCounts[i] = scanner.nextInt();
        }
        System.out.println(new PoemCreator(poem, remainSpaceBarCount, remainLettersCounts).create());
    }
}

class PoemCreator {
    private static final int SPACE_BAR_INDEX = 26;
    private static final String NO_TITLE = "-1";
    private final String poem;
    private StringBuilder title;
    private int[] countsOfLetters;

    public PoemCreator(String poem, int remainSpaceBarCount, int[] remainLettersCounts) {
        this.poem = poem;
        initTitle(poem);
        initCountsOfLetters(remainSpaceBarCount, remainLettersCounts);
    }

    private void initTitle(String poem) {
        title = new StringBuilder();
        title.append(Character.toUpperCase(poem.charAt(0)));
    }

    private void initCountsOfLetters(int remainSpaceBarCount, int[] remainLettersCounts) {
        countsOfLetters = new int[27];
        System.arraycopy(remainLettersCounts, 0, countsOfLetters, 0, 26);
        countsOfLetters[SPACE_BAR_INDEX] = remainSpaceBarCount;
    }

    public String create() {
        try {
            createTitle();
            checkRemainLettersForTitle();
        } catch (NoSuchElementException e) {
            return NO_TITLE;
        }

        return title.toString();
    }

    private void createTitle() {
        char prevLetter = 0;
        for (char currentLetter : poem.toCharArray()) {
            if (isSameAsPreviousCharacter(prevLetter, currentLetter)) {
                continue;
            }
            checkRemainLetters(currentLetter);
            addTitle(prevLetter, currentLetter);
            prevLetter = currentLetter;
        }
    }

    private boolean isSameAsPreviousCharacter(char prevLetter, char currentLetter) {
        return currentLetter == prevLetter;
    }

    private void checkRemainLetters(char currentLetter) {
        int index = getCharacterIndex(currentLetter);
        countsOfLetters[index]--;
        if (countsOfLetters[index] < 0) {
            throw new NoSuchElementException();
        }
    }

    private void addTitle(char prevLetter, char currentLetter) {
        if (getCharacterIndex(prevLetter) == SPACE_BAR_INDEX) {
            title.append(Character.toUpperCase(currentLetter));
        }
    }

    private int getCharacterIndex(char character) {
        if (character == 32) {
            return SPACE_BAR_INDEX;
        }
        return (character >= 65 && character < 91) ? character - 65 : character - 97;
    }

    private void checkRemainLettersForTitle() {
        for (char currentLetter : title.toString().toCharArray()) {
            checkRemainLetters(currentLetter);
        }
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.NoSuchElementException;

import static org.junit.jupiter.api.Assertions.*;

public class MainTest {

    @Test
    void solutionTest() {
        int[] fullCountOfLetters = new int[]{999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999, 999};
        int[] twoCountOfLetters = new int[]{2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2};

        assertEquals("-1", createPoemTitle("a b c", 0, fullCountOfLetters));
        assertEquals("ABC", createPoemTitle("a b c", 999, fullCountOfLetters));
        assertEquals("PE", createPoemTitle("po    em", 999, fullCountOfLetters));
        assertEquals("POEM", createPoemTitle("p o    e    m", 999, fullCountOfLetters));
        assertEquals("POEM", createPoemTitle("p o    e    m", 3, fullCountOfLetters));
        assertEquals("-1", createPoemTitle("p o    e    m", 2, fullCountOfLetters));
        assertEquals("POEM", createPoemTitle("pp o    e    m", 999, twoCountOfLetters));
        assertEquals("-1", createPoemTitle("p o    e    mp", 999, twoCountOfLetters));
        assertEquals("A", createPoemTitle("aaaaaab", 999, twoCountOfLetters));
        assertEquals("-1", createPoemTitle("aaaaaAb", 999, twoCountOfLetters));

        assertEquals("TINCL", createPoemTitle("There is no cow level", 5, new int[]{1, 0, 2, 0, 4, 3, 0, 1, 2, 0, 0, 3, 0, 2, 2, 0, 4, 1, 1, 2, 0, 1, 1, 0, 0, 0}));
        assertEquals("-1", createPoemTitle("Show me the money", 2, new int[]{0, 1, 0, 4, 3, 0, 0, 2, 0, 0, 0, 0, 4, 1, 2, 0, 0, 0, 1, 2, 0, 0, 1, 0, 1, 2}));
        assertEquals("SMTM", createPoemTitle("Show me the money", 3, new int[]{0, 1, 0, 4, 4, 0, 0, 3, 0, 0, 0, 0, 4, 2, 3, 0, 0, 0, 2, 2, 0, 0, 1, 0, 2, 2}));
        assertEquals("-1", createPoemTitle("Show me the money", 4, new int[]{1, 0, 3, 2, 1, 0, 0, 2, 0, 0, 0, 0, 4, 1, 2, 0, 0, 0, 1, 2, 0, 0, 1, 0, 1, 0}));
    }

    private String createPoemTitle(String poem, int remainSpaceBarCount, int[] remainLettersCounts) {
        return new PoemCreator(poem, remainSpaceBarCount, remainLettersCounts).create();
    }

    @Test
    void getCharacterIndexTest() {
        assertEquals(0, getCharacterIndex('a'));
        assertEquals(1, getCharacterIndex('b'));
        assertEquals(0, getCharacterIndex('A'));
        assertEquals(25, getCharacterIndex('Z'));
        assertEquals(26, getCharacterIndex(' '));
    }

    private int getCharacterIndex(char character) {
        if (character == 32) {
            return 26;
        }
        return (character >= 65 && character < 91) ? character - 65 : character - 97;
    }

    static class PoemCreator {
        private static final int SPACE_BAR_INDEX = 26;
        private static final String NO_TITLE = "-1";
        private final String poem;
        private StringBuilder title;
        private int[] countsOfLetters;

        public PoemCreator(String poem, int remainSpaceBarCount, int[] remainLettersCounts) {
            this.poem = poem;
            initTitle(poem);
            initCountsOfLetters(remainSpaceBarCount, remainLettersCounts);
        }

        private void initTitle(String poem) {
            title = new StringBuilder();
            title.append(Character.toUpperCase(poem.charAt(0)));
        }

        private void initCountsOfLetters(int remainSpaceBarCount, int[] remainLettersCounts) {
            countsOfLetters = new int[27];
            System.arraycopy(remainLettersCounts, 0, countsOfLetters, 0, 26);
            countsOfLetters[SPACE_BAR_INDEX] = remainSpaceBarCount;
        }

        public String create() {
            try {
                createTitle();
                checkRemainLettersForTitle();
            } catch (NoSuchElementException e) {
                return NO_TITLE;
            }

            return title.toString();
        }

        private void createTitle() {
            char prevLetter = 0;
            for (char currentLetter : poem.toCharArray()) {
                if (isSameAsPreviousCharacter(prevLetter, currentLetter)) {
                    continue;
                }
                checkRemainLetters(currentLetter);
                addTitle(prevLetter, currentLetter);
                prevLetter = currentLetter;
            }
        }

        private boolean isSameAsPreviousCharacter(char prevLetter, char currentLetter) {
            return currentLetter == prevLetter;
        }

        private void checkRemainLetters(char currentLetter) {
            int index = getCharacterIndex(currentLetter);
            countsOfLetters[index]--;
            if (countsOfLetters[index] < 0) {
                throw new NoSuchElementException();
            }
        }

        private void addTitle(char prevLetter, char currentLetter) {
            if (getCharacterIndex(prevLetter) == SPACE_BAR_INDEX) {
                title.append(Character.toUpperCase(currentLetter));
            }
        }

        private int getCharacterIndex(char character) {
            if (character == 32) {
                return SPACE_BAR_INDEX;
            }
            return (character >= 65 && character < 91) ? character - 65 : character - 97;
        }

        private void checkRemainLettersForTitle() {
            for (char currentLetter : title.toString().toCharArray()) {
                checkRemainLetters(currentLetter);
            }
        }
    }
}
~~~
