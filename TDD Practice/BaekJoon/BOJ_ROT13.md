# ROT13 (Silver 5)

### 문제 설명

간달프는 여러 종족의 언어를 꽤 오랜 시간 동안 공부했다. 최근에 간달프는 해커들이 사용하는 언어인 ROT13을 공부했다. 이 언어는 영어와 문법이 같지만, 알파벳의 순서를 어떤 규칙을 이용해서 바꾸는 것이다.

모음은 총 6개가 있다.

(a i y e o u)

모음은 각 위치에서 세 번째 오른쪽 위치에 있는 모음으로 바꾼다. 위의 수열은 사이클이라서 마지막과 첫 위치는 서로 붙어있는 것이다. 이때, 대소문자는 유지해야 한다.

자음도 모음과 비슷하게 바꾸면 된다.

(b k x z n h d c w g p v j q t s r l m f)

자음은 각 위치에서 열번 위치에 있는 자음으로 바꾼다.

다음과 같은 문장 "One ring to rule them all." ROT13을 이용하면 "Ita dotf ni dyca nsaw ecc."으로 바꿀 수 있다.

ROT13으로 쓴 문장이 주어졌을 때, 원래 영어로 쓴 문장으로 바꾸는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/4446

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder encryptedString = new StringBuilder();
        while (scanner.hasNext()) {
            encryptedString.append(scanner.nextLine());
            encryptedString.append('\n');
        }
        System.out.println(new Rot13().decrypt(encryptedString.toString()));
    }
}

class Rot13 {
    private static final List<Character> VOWELS = List.of('a', 'i', 'y', 'e', 'o', 'u');
    private static final List<Character> CONSONANTS = List.of('b', 'k', 'x', 'z', 'n', 'h', 'd', 'c', 'w', 'g', 'p', 'v', 'j', 'q', 't', 's', 'r', 'l', 'm', 'f');
    private static final int VOWEL_SALT = 3;
    private static final int CONSONANT_SALT = 10;

    public String decrypt(String encryptedString) {
        StringBuilder decryptedString = new StringBuilder();
        for (char letter : encryptedString.toCharArray()) {
            decryptedString.append(decryptLetter(letter));
        }
        return decryptedString.toString();
    }

    private char decryptLetter(char letter) {
        if (!Character.isAlphabetic(letter)) {
            return letter;
        }
        return decryptAlphabetLetter(letter);
    }

    private char decryptAlphabetLetter(char letter) {
        if (VOWELS.contains(Character.toLowerCase(letter))) {
            return decryptVowels(letter);
        }
        return decryptConsonants(letter);
    }

    private char decryptVowels(char letter) {
        if (Character.isUpperCase(letter)) {
            return Character.toUpperCase(VOWELS.get(getVowelsIndex(Character.toLowerCase(letter))));
        }
        return VOWELS.get(getVowelsIndex(letter));
    }

    private int getVowelsIndex(char letter) {
        return (VOWELS.indexOf(letter) + VOWEL_SALT) % VOWELS.size();
    }

    private char decryptConsonants(char letter) {
        if (Character.isUpperCase(letter)) {
            return Character.toUpperCase(CONSONANTS.get(getConsonantsIndex(Character.toLowerCase(letter))));
        }
        return CONSONANTS.get(getConsonantsIndex(letter));
    }

    private int getConsonantsIndex(char letter) {
        return (CONSONANTS.indexOf(letter) + CONSONANT_SALT) % CONSONANTS.size();
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void rot13Test() {
        Rot13 rot13 = new Rot13();

        assertEquals("e", rot13.decrypt("a"));
        assertEquals("o", rot13.decrypt("i"));
        assertEquals("u", rot13.decrypt("y"));
        assertEquals("a", rot13.decrypt("e"));
        assertEquals("y", rot13.decrypt("u"));
        assertEquals("eie", rot13.decrypt("aoa"));
        assertEquals("ueu", rot13.decrypt("yay"));
        assertEquals("eie ueu", rot13.decrypt("aoa yay"));
        assertEquals("E", rot13.decrypt("A"));
        assertEquals("EIE ueu", rot13.decrypt("AOA yay"));
        assertEquals("EIE UEU", rot13.decrypt("AOA YAY"));
        assertEquals("p", rot13.decrypt("b"));
        assertEquals("n", rot13.decrypt("t"));
        assertEquals("One ring to rule them all.", rot13.decrypt("Ita dotf ni dyca nsaw ecc."));
        assertEquals("One ring to rule them all.\neie", rot13.decrypt("Ita dotf ni dyca nsaw ecc.\naoa"));
        assertEquals("!@#$", rot13.decrypt("!@#$"));
    }

    static class Rot13 {
        private static final List<Character> VOWELS = List.of('a', 'i', 'y', 'e', 'o', 'u');
        private static final List<Character> CONSONANTS = List.of('b', 'k', 'x', 'z', 'n', 'h', 'd', 'c', 'w', 'g', 'p', 'v', 'j', 'q', 't', 's', 'r', 'l', 'm', 'f');
        private static final int VOWEL_SALT = 3;
        private static final int CONSONANT_SALT = 10;

        public String decrypt(String encryptedString) {
            StringBuilder decryptedString = new StringBuilder();
            for (char letter : encryptedString.toCharArray()) {
                decryptedString.append(decryptLetter(letter));
            }
            return decryptedString.toString();
        }

        private char decryptLetter(char letter) {
            if (!Character.isAlphabetic(letter)) {
                return letter;
            }
            return decryptAlphabetLetter(letter);
        }

        private char decryptAlphabetLetter(char letter) {
            if (VOWELS.contains(Character.toLowerCase(letter))) {
                return decryptVowels(letter);
            }
            return decryptConsonants(letter);
        }

        private char decryptVowels(char letter) {
            if (Character.isUpperCase(letter)) {
                return Character.toUpperCase(VOWELS.get(getVowelsIndex(Character.toLowerCase(letter))));
            }
            return VOWELS.get(getVowelsIndex(letter));
        }

        private int getVowelsIndex(char letter) {
            return (VOWELS.indexOf(letter) + VOWEL_SALT) % VOWELS.size();
        }

        private char decryptConsonants(char letter) {
            if (Character.isUpperCase(letter)) {
                return Character.toUpperCase(CONSONANTS.get(getConsonantsIndex(Character.toLowerCase(letter))));
            }
            return CONSONANTS.get(getConsonantsIndex(letter));
        }

        private int getConsonantsIndex(char letter) {
            return (CONSONANTS.indexOf(letter) + CONSONANT_SALT) % CONSONANTS.size();
        }
    }
}
~~~
