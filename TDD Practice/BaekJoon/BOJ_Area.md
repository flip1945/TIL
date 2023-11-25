# Area (Bronze 4)

### 문제 설명

Stuart has $n$ rectangular frames, which are numbered from $1$ to $n$. Frame $i$ is a rectangle with height $h[i]$ and width $w[i]$.

The size of a frame is the area that it covers. Stuart wants you to help him find the area covered by the largest size frame that he has.

출처 : https://www.acmicpc.net/problem/11170

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();

        Frames frames = new Frames(inputFrames(scanner, n));
        System.out.println(frames.largestFrameArea());
    }

    private static List<Frame> inputFrames(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> new Frame(scanner.nextInt(), scanner.nextInt()))
                .collect(Collectors.toList());
    }
}

class Frames {

    private final List<Frame> frames;

    public Frames(List<Frame> frames) {
        this.frames = frames;
    }

    public int largestFrameArea() {
        return frames.stream()
                .map(Frame::getArea)
                .max(Comparator.naturalOrder())
                .orElseThrow();
    }
}

class Frame {

    private final int height;
    private final int width;

    public Frame(int height, int width) {
        this.height = height;
        this.width = width;
    }

    public int getArea() {
        return height * width;
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

import java.util.Comparator;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideHeightAndWidth")
    @DisplayName("높이와 너비가 주어지면 면적을 반환한다.")
    void frameReturnsArea(int height, int width, int expected) {
        Frame sut = new Frame(height, width);

        int actual = sut.getArea();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideHeightAndWidth() {
        return Stream.of(
                Arguments.of(5, 9, 45),
                Arguments.of(19, 4, 76),
                Arguments.of(1, 0, 0)
        );
    }

    @ParameterizedTest
    @MethodSource("provideFrames")
    @DisplayName("frame 목록이 주어지면 가장 면적이 넓은 frame의 면적을 반환한다.")
    void framesReturnsLargestAreaFrame(List<Frame> frames, int expected) {
        Frames sut = new Frames(frames);

        int actual = sut.largestFrameArea();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideFrames() {
        return Stream.of(
                Arguments.of(List.of(new Frame(1, 2)), 2),
                Arguments.of(List.of(new Frame(5, 9), new Frame(19, 4), new Frame(8, 10)), 80)
        );
    }
}

class Frames {

    private final List<Frame> frames;

    public Frames(List<Frame> frames) {
        this.frames = frames;
    }

    public int largestFrameArea() {
        return frames.stream()
                .map(Frame::getArea)
                .max(Comparator.naturalOrder())
                .orElseThrow();
    }
}

class Frame {

    private final int height;
    private final int width;

    public Frame(int height, int width) {
        this.height = height;
        this.width = width;
    }

    public int getArea() {
        return height * width;
    }
}
~~~
