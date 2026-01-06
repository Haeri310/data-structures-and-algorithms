# MST

## 문제분석
- 최소의 비용으로 모든 노드가 연결된 트리
- Kruskal: 전체 간선 중 작은 것부터 연결
- Prim: 현재 연결된 트리에 이어진 간선 중 가장 작은 것을 추가
- Prim이 비교적 구현이 쉬운편이다
- MST 문제 유형: 모든 노드가 연결되도록 / 이미 연결된 노드를 최소 비용으로 줄이기

## 알고리즘
- 간선을 인접리스트 형태로 저장한다
- 시작점을 Priority Queue 에 넣는다
- Priority Queue 가 빌 때까지, 방문여부 체크, 비용 추가, 연결된 간선 새롭게 추가
- O(VlogE) - 모든 간선에 관하여 노드 추가, 노드 제거

## 의사코드

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


