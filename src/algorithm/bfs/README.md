# BFS

## 문제분석
- 지도에서 탐색을 하며 __최단거리__ 를 찾는다

## 알고리즘
- 다음 노드로 이동하며 방문한 적이 있는지 체크하고, 방문한 적이 없다면 visited 배열에 이동거리를 적고 해당 좌표로 이동한다
- 해당 노드에 처음 방문할 때가 가장 적은 이동거리를 가지기 때문에, 방문체크만 하면 된다 (이동거리의 비교는 불필요하다)
- O(Vertex + Edge) 이동한 거리가 곧 시간복잡도이다 (column * row 로 계산할수도있다)

## 의사코드

```
int[][] map = new int[N][M];
int[][] visited = new visited[N][M];

int[] dy = {1, 0, -1, 0};
int[] dx = {0, 1, 0, -1};

void bfs(int y, int x) {
  Queue<Point> queue = new LinkedList<>();
  queue.add(new Point(y, x));
  visited[y][x] = 1;

  while (!queue.isEmpty()) {
    Point now = queue.poll();
    for (int d=0; d<4; d++) {
      int nextY = now.y + dy[d];
      int nextX = now.x + dx[d];

      if (nextY < 0 || nextY >= N || nextX < 0 || nextX >= M) {
        continue;
      }
      if (visited[nextY][nextX] > 0) {
        continue;
      }
      queue.add(new Point(nextY, nextX));
      visited[nextY][nextX] = vistied[now.y][nox.x] + 1;
    }
  }
}
```

## 심화 아이디어
- `int[][][] visited = new int[N][M][2];` visited 배열을 3차원, 4차원 해야할 경우도 있다
- 예를들어 지도 뿐 아니라, 해당 좌표로 가기 위해 열쇠가 필요하거나, 벽 부수기 이용권이 필요하거나, 무언가 아이템이 필요하다면 아이템을 쓰고 방문한 경우와 쓰지 않고 방문한 경우를 구분할 필요가 있다.
