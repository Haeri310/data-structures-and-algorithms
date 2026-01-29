# 위상정렬 (Topological Sort)

## ✅ 문제분석
- A 가 끝나야 B 를 할 수 있다 / 선수과목 / 작업순서 / 의존성
- 단방향 그래프 + 비순환그래프

## ✅ 알고리즘
1. 각 노드마다 연결된 간선의 개수(indegree) 를 입력한다
2. indegree 가 0인 간선부터 Queue 에 넣어 Queue 가 빌 때까지 아래 과정을 반복한다
3. 해당 노드와 연결된 노드들의 indegree 를 -1 해주고, indegree 가 0 이되면 해당 노드의 선행조건이 만족됨으로 Queue 에 추가한다

### 시간복잡도: O(V + E)
- 모든 노드에 관하여 순회하며 indegree 가 0 인 노드를 찾은 후, 모든 간선을 돌면서 indegree-- 해준다

## ✅ 의사코드

```java
public class BOJ2056 {

    private static int N;

    private static int[] times;
    private static int[] indegree;
    private static List<Integer>[] graph;

    public static void main(String[] args) throws IOException {
        setup();
        int[] result = topologicalSort();
    }

    private static void setup() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        times = new int[N+1];
        indegree = new int[N+1];
        graph = new ArrayList[N+1];
        for (int i=1; i<=N; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int to=1; to<=N; to++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            times[to] = Integer.parseInt(st.nextToken());
            indegree[to] = Integer.parseInt(st.nextToken());

            for (int i=0; i<indegree[to]; i++) {
                int from = Integer.parseInt(st.nextToken());
                graph[from].add(to);
            }
        }
    }

    private static int[] topologicalSort() {
        Queue<Integer> queue = new LinkedList<>();

        int[] distances = new int[N+1];

        //조건 없이 처음부터 수행가능한 노드들을 Queue 에 삽입한다
        for (int i=1; i<=N; i++) {
            if (indegree[i] == 0) {
                distances[i] = times[i];
                queue.add(i);
            }
        }

        while (!queue.isEmpty()) {
            int curr = queue.poll();

            for (int next : graph[curr]) {
                indegree[next]--;
                //만약 indegree == 0 일 때에만 아래 코드를 실행한다면, 해당 노드로 이동하는 경로가 여러 개인 경우 마지막 경로의 시간으로 입력되어 버린다.
                //매번 최댓값으로 설정하면 자연스럽게 해당 노드에 도달하기에 소요되는 시간 중 최댓값으로 설정된다 
                distances[next] = Math.max(distances[next], distances[curr] + times[next]);
                //선행조건을 만족한 노드를 Queue 에 넣는다
                if (indegree[next] == 0) {
                    queue.add(next);
                }
            }
        }

        return distances;
    }
}
```
