# MP3 Songs (Silver 5)

### 문제 설명

Sue loves her mp3 player but she hates the fact that her shuffle mode plays her tracks randomly. Because she loves order and patterns she would like the tunes on her mp3 player to be played in alphabetical order. In this problem you have to help Sue by sorting her tunes into alphabetical order of tune name. 

출처 : https://www.acmicpc.net/problem/7596

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int index = 1;

        int n = Integer.parseInt(scanner.nextLine());
        while (n != 0) {
            Playlist originPlaylist = Playlist.from(inputSongs(scanner, n));
            Playlist sortedPlaylist = originPlaylist.sort();

            System.out.println(index);
            System.out.println(sortedPlaylist);

            n = Integer.parseInt(scanner.nextLine());
            index++;
        }
    }

    private static List<String> inputSongs(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Playlist {

    private final List<Song> songs;

    public Playlist(List<Song> songs) {
        this.songs = songs;
    }

    public static Playlist from(List<String> songs) {
        return new Playlist(initPlaylist(songs));
    }

    private static List<Song> initPlaylist(List<String> songs) {
        return songs.stream()
                .map(Song::new)
                .collect(Collectors.toList());
    }

    public Playlist sort() {
        return new Playlist(getSortedSongs());
    }

    private List<Song> getSortedSongs() {
        return songs.stream()
                .sorted()
                .collect(Collectors.toList());
    }

    @Override
    public String toString() {
        return songs.stream()
                .map(Song::getTitle)
                .collect(Collectors.joining("\n"));
    }
}

class Song implements Comparable<Song> {

    private final String title;

    public Song(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }

    @Override
    public int compareTo(Song o) {
        return title.compareTo(o.title);
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

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSongs")
    @DisplayName("플레이리스트는 노래를 정렬한다.")
    void playlistSortSongs(List<String> songs, String expected) {
        Playlist sut = Playlist.from(songs);

        String actual = sut.sort().toString();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSongs() {
        return Stream.of(
                Arguments.of(List.of("C", "B", "A"), "A\nB\nC"),
                Arguments.of(List.of("cat", "apple", "bat"), "apple\nbat\ncat"),
                Arguments.of(
                        List.of("Forever", "Take A Bow", "Always On My Mind", "Lollipop", "Love In This Club", "No Air",
                                "Sweet About Me", "Party People", "Mercy", "American Boy"),
                        "Always On My Mind\nAmerican Boy\nForever\nLollipop\nLove In This Club\nMercy\nNo Air\n" +
                                "Party People\nSweet About Me\nTake A Bow"),
                Arguments.of(
                        List.of("Partita", "Air on a 'G' string", "Sinfonia in D", "Jesu, joy of man's desiring", "Arioso",
                                "Violin Concerto in A Minor", "Brandenburg Concerto 2", "Concerto for 2 violins"),
                        "Air on a 'G' string\nArioso\nBrandenburg Concerto 2\nConcerto for 2 violins\n" +
                                "Jesu, joy of man's desiring\nPartita\nSinfonia in D\nViolin Concerto in A Minor")
        );
    }
}

class Playlist {

    private final List<Song> songs;

    public Playlist(List<Song> songs) {
        this.songs = songs;
    }

    public static Playlist from(List<String> songs) {
        return new Playlist(initPlaylist(songs));
    }

    private static List<Song> initPlaylist(List<String> songs) {
        return songs.stream()
                .map(Song::new)
                .collect(Collectors.toList());
    }

    public Playlist sort() {
        return new Playlist(getSortedSongs());
    }

    private List<Song> getSortedSongs() {
        return songs.stream()
                .sorted()
                .collect(Collectors.toList());
    }

    @Override
    public String toString() {
        return songs.stream()
                .map(Song::getTitle)
                .collect(Collectors.joining("\n"));
    }
}

class Song implements Comparable<Song> {

    private final String title;

    public Song(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }

    @Override
    public int compareTo(Song o) {
        return title.compareTo(o.title);
    }
}
~~~
