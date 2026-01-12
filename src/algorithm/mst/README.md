# MST

## 문제분석
- 최소 혹은 최대의 비용으로 모든 노드가 연결된 트리
- 최소 혹은 최대 비용의 순서대로 간선들을 이으며 특정 조건을 만족하는지 체크
- Compare 조건 설정에 따라 최대도 가능하다는 점 꼭 기억하자 !!

## 알고리즘

| 구분 | Prim (프림) | Kruskal (크루스칼) |
| --- | --- | --- |
| 핵심 아이디어 | 정점(Vertex) 중심. 시작점에서 가까운 정점을 선택하며 확장한다 | 간선(Edge) 중심. 가중치가 작은 간선부터 연결하며 확장한다 |
| 시간 복잡도 | O(E * log(V)) [연결된 간선을 확인하고 우선순위 큐에 값을 넣는다]| O(E * log(E)) [정렬시간]|
| 사용하는 경우 | 간선이 많을 때 | 간선이 적을 때 |
| 구현 도구 | 우선순위 큐(Heap) | Union-Find, 정렬(Sorting) |



### Prim vs Kruskal 어느 것을 선택해야 할까?
1. 시작점 기준이 없이 모든 노드를 연결할 경우 --> Kruskal
2. 시작점 도착점 기준이 존재하는 경우 --> Prim

> 물론 시작점 도착점이 있어도 Kruskal 로 풀 수 있고, 없더라도 Prim 으로 풀 수 있다. 생각하기 편한 걸로 풀면 된다

### 시간복잡도 비교
- 시간복잡도는 사실 비슷하다. 간선의 개수 E는 최대 V^2 개다. <br>
- Kruskal 시간복잡도 = E*log(E) = E*log(V^2) = 2E*log(V) = Prim 시간복잡도 <br>
- 시간복잡도는 최대 2배 차이기에, 설령 최대 간선(E = V^2) 이라고 해도 별로 상관 없다

## Prim 의사코드

```java
public class BOJ1197 {

    static class Node implements Comparable<Node> {
        int to;
        int weight;

        public Node(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }

        //오름차순
        @Override
        public int compareTo(Node o) {
            return this.weight - o.weight;
        }
    }

    private static int V; //정점 개수
    private static int E; //간선 개수
    private static List<Node>[] graph;


    public static void main(String[] args) throws IOException {
        setup();
        long result = mst();
        System.out.println(result);
    }

    private static void setup() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());

        graph = new ArrayList[V+1]; //ArrayList 로 하지 않고 int[] 로 하면 크기가 너무 커져서 메모리초과 리스크가 존재한다
        for (int i=1; i<=V; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int i=0; i<E; i++) {
            st = new StringTokenizer(br.readLine());
            int A = Integer.parseInt(st.nextToken());
            int B = Integer.parseInt(st.nextToken());
            int C = Integer.parseInt(st.nextToken());
            graph[A].add(new Node(B, C));
            graph[B].add(new Node(A, C));
        }
    }

    private static long mst() {
        long totalWeight = 0;

        boolean[] visited = new boolean[V+1];
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.add(new Node(1, 0));

        while (!pq.isEmpty()) {
            Node current = pq.poll();

            //방문체크 후 노드를 꺼내면서 가중치 합산
            if (visited[current.to]) {
                continue;
            }
            visited[current.to] = true;
            totalWeight += current.weight;

            //방문체크 후 Priority Queue 에 다음 노드 삽입
            for (Node next : graph[current.to]) {
                if (visited[next.to]) {
                    continue;
                }
                pq.add(next);
            }
        }

        return totalWeight;
    }
}
```

## Kruskal 의사코드

```java
public class BOJ1197 {

    static class Edge implements Comparable<Edge> {
        int from;
        int to;
        int weight;

        public Edge (int from, int to, int weight) {
            this.from = from;
            this.to = to;
            this.weight = weight;
        }

        @Override
        public int compareTo(Edge o) {
            return this.weight - o.weight;
        }
    }

    private static int V;
    private static int E;
    private static List<Edge> edges;
    private static int[] parents;

    public static void main(String[] args) throws IOException {
        setup();
        long result = mst();
        System.out.println(result);
    }

    private static void setup() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());

        parents = new int[V + 1];
        for (int i=1; i<=V; i++) {
            parents[i] = i;
        }

        edges = new ArrayList<>();
        for (int i=0; i<E; i++) {
            st = new StringTokenizer(br.readLine());
            int A = Integer.parseInt(st.nextToken());
            int B = Integer.parseInt(st.nextToken());
            int C = Integer.parseInt(st.nextToken());
            edges.add(new Edge(A, B, C));
        }
    }

    private static long mst() {
        long totalWeight = 0;
        Collections.sort(edges);

        for (Edge edge : edges) {
            if (union(edge.from, edge.to)) {
                totalWeight += edge.weight;
            }
        }
        return totalWeight;
    }

    private static boolean union(int y, int x) {
        y = find(y);
        x = find(x);

        if (y == x) {
            return false;
        }

        if (y < x) {
            parents[x] = y;
        } else if (y > x) {
            parents[y] = x;
        }
        return true;
    }

    private static int find(int x) {
        if (parents[x] != x) {
            parents[x] = find(parents[x]); //경로 압축
            return parents[x];
        }
        return x;
    }
}
```
