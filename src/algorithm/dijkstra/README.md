# 다익스트라 (Dijkstra's)

## 문제분석
- 음의 가중치가 없는 그래프에서 하나의 노드에서 다른 모든 노드까지 가는 최소 비용을 구한다

## 알고리즘

### 작동방식

```java
if (distance[next.to] > distance[curr.to] + next.weight) {
    distance[next.to] = distance[curr.to] + next.weight;
    queue.add(new Node(next.to, distance[next.to]));
}
```
PriorityQueue 에 다음 노드와 시작점~목표점 까지의 총 가중치를 추가한다. 
총 가중치로 Node 를 넣는 이유는 PriorityQueue 가 다음노드로 가는 Edge 중에 가장 짧은 가중치를 기준으로 방문시키게 하기 위함이다 

```java
if (visited[curr.to]) {
    continue;
}
visited[curr.to] = true;
```
해당 노드에 방문시 visited 체크를 한다. PriorityQueue 는 총 가중치를 기준으로 정렬이 되기에, 해당 노드를 방문하는 데에 최소 가중치로 방문할 수 있다

위 과정을 반복하며 시작점으로부터 가장 가중치가 적게 걸리는 순서대로 노드들을 방문하며, 방문체크와 거리체크를 반복한다.

### 시간복잡도 
- O(E * log(V))
- 다음 간선(E)에 대해 반복문으로 거리를 재며 검사를 하고, 조건에 맞으면 Priority Queue 에 노드(V)를 추가한다 --> E * log(V) 

## 의사코드

```java
public class BOJ1753 {

    static class Node implements Comparable<Node> {
        int to;
        int weight;

        public Node(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }

        @Override
        public int compareTo(Node o) {
            return this.weight - o.weight;
        }
    }

    private static int V; // 정점
    private static int E; // 간선
    private static int K; // 시작점

    private static List<Node>[] graph;


    public static void main(String[] args) throws IOException {
        setup();
        int[] result = dijkstra();
        for (int i=1; i<=V; i++) {
            if (result[i] == Integer.MAX_VALUE) {
                System.out.println("INF");
            } else {
                System.out.println(result[i]);
            }
        }
    }

    private static void setup() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(br.readLine());

        graph = new ArrayList[V+1];
        for (int i=1; i<=V; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int i=0; i<E; i++) {
            st = new StringTokenizer(br.readLine());
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());
            graph[u].add(new Node(v, w));
        }
    }

    private static int[] dijkstra() {
        int[] distance = new int[V+1];
        Arrays.fill(distance, Integer.MAX_VALUE);

        boolean[] visited = new boolean[V + 1];

        PriorityQueue<Node> queue = new PriorityQueue<>();
        queue.add(new Node(K, 0));
        distance[K] = 0;

        while (!queue.isEmpty()) {
            Node curr = queue.poll();

            if (visited[curr.to]) {
                continue;
            }
            visited[curr.to] = true;

            for (Node next : graph[curr.to]) {
                if (visited[next.to]) {
                    continue;
                }

                if (distance[next.to] > distance[curr.to] + next.weight) {
                    distance[next.to] = distance[curr.to] + next.weight;
                    queue.add(new Node(next.to, distance[next.to]));
                }
            }
        }
        return distance;
    }
}
```
