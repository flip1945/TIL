# 파일 정리 (Silver 3)

### 문제 설명

친구로부터 노트북을 중고로 산 스브러스는 노트북을 켜자마자 경악할 수밖에 없었다. 바탕화면에 온갖 파일들이 정리도 안 된 채 가득했기 때문이다. 그리고 화면의 구석에서 친구의 메시지를 확인할 수 있었다.

~~~
바탕화면의 파일들에는 값진 보물에 대한 정보가 들어 있어.
하나라도 지우게 된다면 보물은 물론이고 다시는 노트북을 쓸 수 없게 될 거야.
파일들을 잘 분석해서 보물의 주인공이 될 수 있길 바랄게. 힌트는 “확장자”야.
~~~

화가 났던 스브러스는 보물 이야기에 금세 화가 풀렸고 보물의 정보를 알아내려고 애썼다. 하지만 파일이 너무 많은 탓에 이내 포기했고 보물의 절반을 보상으로 파일의 정리를 요청해왔다. 스브러스의 요청은 다음과 같다.

* 파일을 확장자 별로 정리해서 몇 개씩 있는지 알려줘
* 보기 편하게 확장자들을 사전 순으로 정렬해 줘

그럼 보물의 절반을 얻어내기 위해 얼른 스브러스의 노트북 파일 정리를 해줄 프로그램을 만들자!

출처 : https://www.acmicpc.net/problem/20291

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> files = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        organizeFile(files).entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .forEach(entry -> System.out.println(entry.getKey() + " " + entry.getValue()));
    }

    static Map<String, Integer> organizeFile(List<String> files) {
        Map<String, Integer> result = new HashMap<>();
        for (String file : files) {
            String fileExtension = getFileExtension(file);
            int count = result.getOrDefault(fileExtension, 0);
            result.put(fileExtension, count + 1);
        }
        return result;
    }

    static String getFileExtension(String file) {
        return file.split("\\.")[1];
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getFileExtensionTest() {
        assertEquals("a", getFileExtension("a.a"));
        assertEquals("text", getFileExtension("ab.text"));
        assertEquals("txt", getFileExtension("abrus.txt"));
        assertEquals("spc", getFileExtension("spc.spc"));
        assertEquals("icpc", getFileExtension("acm.icpc"));
        assertEquals("icpc", getFileExtension("korea.icpc"));
        assertEquals("txt", getFileExtension("sample.txt"));
        assertEquals("world", getFileExtension("hello.world"));
    }

    @Test
    void organizeFileTest() {
        List<String> files1 = List.of("a.a", "b.b", "c.c");
        Map<String, Integer> organizedFile1 = organizeFile(files1);
        assertEquals(1, organizedFile1.get("a"));
        assertEquals(1, organizedFile1.get("b"));
        assertEquals(1, organizedFile1.get("c"));

        List<String> files2 = List.of("a.a", "b.b", "c.a");
        Map<String, Integer> organizedFile2 = organizeFile(files2);
        assertEquals(2, organizedFile2.get("a"));
        assertEquals(1, organizedFile2.get("b"));

        List<String> files3 = List.of("test.text");
        Map<String, Integer> organizedFile3 = organizeFile(files3);
        assertEquals(1, organizedFile3.get("text"));

        List<String> files4 = List.of("sbrus.txt", "spc.spc", "acm.icpc", "korea.icpc", "sample.txt", "hello.world", "sogang.spc", "example.txt");
        Map<String, Integer> organizedFile4 = organizeFile(files4);
        assertEquals(2, organizedFile4.get("icpc"));
        assertEquals(2, organizedFile4.get("spc"));
        assertEquals(3, organizedFile4.get("txt"));
        assertEquals(1, organizedFile4.get("world"));
    }

    private Map<String, Integer> organizeFile(List<String> files) {
        Map<String, Integer> result = new HashMap<>();
        for (String file : files) {
            String fileExtension = getFileExtension(file);
            int count = result.getOrDefault(fileExtension, 0);
            result.put(fileExtension, count + 1);
        }
        return result;
    }

    private String getFileExtension(String file) {
        return file.split("\\.")[1];
    }
}
~~~
