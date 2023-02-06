# 연구소3
## 문제를 풀기 위해서 한 생각
1. 시간이 0.25초로 1초에 1억번 연산할 수 있을 경우 2500만번 연산이 가능하다. 
2. 무작위 바이러스 10개를 배치하는 최악의 연산은 10C5이다.
3. 입력 배열의 크기는 50 x 50이 최대이다.<br>
```
조합을 이용해서 바이러스의 위치를 뽑은 후 bfs를 하면 문제를 풀이 할 수 있겠다고 생각
```
## 코드
[코드 링크](https://github.com/eui20n/Algorithm_JAVA/blob/master/src/baekjoon/t17000f17999/p17142/Main.java)

<details>
<summary>코드 펼치기</summary>
<div markdown="1">

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Main {
    static int N;
    static int M;
    static int spread;
    static int ans = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] tmp = br.readLine().split(" ");
        N = Integer.parseInt(tmp[0]);
        M = Integer.parseInt(tmp[1]);

        int[][] arr = new int[N][N];
        List<int[]> virus = new ArrayList<>();

        for (int i = 0; i < N; i++) {
            String[] tmp2 = br.readLine().split(" ");
            for (int j = 0; j < N; j++) {
                if (tmp2[j].equals("2")) {
                    int[] virusLoc = { i, j };
                    virus.add(virusLoc);
                    arr[i][j] = 1;
                } else if (tmp2[j].equals("0")) {
                    spread += 1;
                    arr[i][j] = 0;
                } else {
                    arr[i][j] = -1;
                }
            }
        }

        checkVirus(arr, virus, 0, 0, new boolean[virus.size()]);
        System.out.println((ans == Integer.MAX_VALUE) ? -1 : ans);

    }

    static void checkVirus(int[][] arr, List<int[]> virus, int cnt, int start, boolean[] visited) {
        if (cnt == M) {
            bfs(arr, virus, visited);
            return;
        }
        for (int i = start; i < virus.size(); i++) {
            visited[i] = true;
            checkVirus(arr, virus, cnt + 1, i + 1, visited);
            visited[i] = false;
        }
    }

    static void bfs(int[][] arr, List<int[]> virus, boolean[] visited) {
        int[] dx = { -1, 1, 0, 0 };
        int[] dy = { 0, 0, -1, 1 };

        boolean[][] bfsVisited = new boolean[N][N];
        int result = 0;
        int checkSpread = 0;

        Queue<int[]> q = new LinkedList<>();
        for (int i = 0; i < visited.length; i++) {
            if (visited[i]) {
                int x = virus.get(i)[0];
                int y = virus.get(i)[1];
                bfsVisited[x][y] = true;
                int[] add = { x, y, 0 };
                q.add(add);
            }
        }

        while (true) {
            if (q.isEmpty())
                break;

            if (ans <= result)
                break;

            if (checkSpread == spread)
                break;

            int[] p = q.poll();
            int x = p[0];
            int y = p[1];
            int time = p[2];

            for (int z = 0; z < 4; z++) {
                int nx = x + dx[z];
                int ny = y + dy[z];
                if (0 > nx || nx >= N)
                    continue;
                if (0 > ny || ny >= N)
                    continue;
                if (bfsVisited[nx][ny])
                    continue;
                if (arr[nx][ny] == -1)
                    continue;
                if (arr[nx][ny] != 1) {
                    checkSpread += 1;

                }
                result = Math.max(result, time + 1);
                q.add(new int[] { nx, ny, time + 1 });
                bfsVisited[nx][ny] = true;
            }
        }

        if (checkSpread == spread) {
            ans = Math.min(result, ans);
        }

    }

}
```
</div>
</details>
