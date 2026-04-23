외판원 순회 문제 (모든 도시를 정확히 한 번씩만 방문하고 처음으로 되돌아오는 거대한 하나의 사이클을 찾는 알고리즘)
만약, 외판원이 돌아오지 않는다면 해밀턴 경로라고 하지만, 그래도 문제 푸는 방식은 거의 같다.
### 시간 복잡도
O(2<sup>N</sup> * N<sup>2</sup>) (N의 범위가 16 이하로 주어지면 의심하자)

상태의 개수: N * 2<sup>N</sup> (now N개 * visited 2<sup>N</sup>개(N개의 비트가 꺼졌다 켜졌다 할 수 있기 때문))
각 상태에서의 루프: N (방문하지 않은 다음 도시 탐색 최대 N번)

## 핵심 아이디어
DP + 비트마스킹
### 비트마스킹
4개의 도시가 있을 때, 1,3번 도시 방문하면 1010
- 방문 체크: `visited & (1 << next)` 가 0이 아니면 이미 방문한 것.
- 방문 표시: `visited | (1 << next)` 를 통해 다음 상태로 전달.
### DP
`dp[now][visited]` 
=> 현재 now 도시에 있고, 지금까지 방문한 도시 집합이 visited일 때, 나머지 도시들을 모두 방문하고 다시 출발점으로 돌아가는 데 드는 최소 비용

```java
import java.util.*;

public class Main {
    static int N;
    static int[][] W;
    static int[][] dp;
    static final int INF = 16_000_000; // N(16) * 최대비용(1,000,000)

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        W = new int[N][N];

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                W[i][j] = sc.nextInt();
            }
        }

        // dp[현재도시][방문상태]
        dp = new int[N][(1 << N)];
        for (int i = 0; i < N; i++) {
	        // 아직 방문하지 않은 상태를 -1로 초기화
            Arrays.fill(dp[i], -1);
        }

        // 0번 도시에서 출발, 0000001로 시작
        System.out.println(tsp(0, 1));
    }

    static int tsp(int now, int visited) {
        // 모든 도시를 다 방문한 경우
        if (visited == (1 << N) - 1) {
            // 출발지(0번)로 돌아갈 수 있는지 확인
            if (W[now][0] == 0) return INF;
            return W[now][0];
            // 해밀턴 문제라면 return 0;하고 끝남
        }

        // 이미 계산한 적이 있다면 반환
        // 이게 없으면 그냥 완전 탐색이 되어버림
        if (dp[now][visited] != -1) {
            return dp[now][visited];
        }

        dp[now][visited] = INF;

        // 방문하지 않은 다음 도시 탐색
        for (int next = 0; next < N; next++) {
            // 길이 없거나(0), 이미 방문했다면 스킵
            if (W[now][next] == 0 || (visited & (1 << next)) != 0) {
                continue;
            }

            // 최소 비용 갱신
            // 현재까지의 최소 비용과 다음 도시로 이동했을 때의 결과 중 작은 값을 선택
            dp[now][visited] = Math.min(dp[now][visited], tsp(next, visited | (1 << next)) + W[now][next]);
        }

        return dp[now][visited];
    }
}
```