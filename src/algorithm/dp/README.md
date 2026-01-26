# DP

## ✅ Bottom-Up vs Top-Down

### 시간복잡도
- 시간복잡도는 둘 다 O(N) ~ O(N^2) 으로 동일하게 설계된다

### 메모리 사용 측면: Bottom-UP
- Bottom-Up 은 반복문이기에 가볍지만, Top-Down 은 재귀방식으로 함수를 여러 번 호출하기에 메모리를 더 많이쓴다
- Bottom-Up 은 배열을 0번부터 순차적으로 채우기에 CPU 캐시 활용도가 좋지만, Top-Down 은 호출 순서에 따라 메모리를 비연속적으로 방문하기에 캐시 효율이 떨어진다

### 누가 더 적게 계산하는가? Top-Down
- Bottom-Up:테이블의 모든 칸을 순서대로 전부 채우기에, 무조건 전체 계산이다
- Top-Down: 재귀로 내려가다가 답을 구하는 데에 필요 없는 경로는 방문하지 않는다
- 예를들어 100 x 100 격자에서 특정 지점까지 가는 경로는 찾는데, 장애물 때문에 갈 수 없는 구역이 많다고 해보자

#### Bottom-Up: 100 x 100 을 2중 for 문으로 전부 돈다
```java
for (int j=0; j<100; j++) {
    for (int i=0; i<100; i++) {
        if (grid[j][i] == OBSTACLE {
            continue;
        }
        dp[j][i] = dp[j-1][i] + dp[j][i+1];
    }
}
```

#### Top-Down: 갈 수 있는 경로만 재귀로 파고들기에 실제 연산 횟수가 더 적다
```java
if (grid[j][i] == OBSTACLE) {
    return 0;
}
return recur(j-1, i) + recur(j, i-1);
```

### 결론
- 모든 칸을 채워야 할 경우 [Bottom-Up] : LIS, 배낭문제(Knapsack), 격자형 DP
- 건너뛰는 칸이 많은 경우 [Top-Down] : 게임이론(승자예측), 복잡한 트리 DP, 상태가 비트마스킹


## ✅ 문제분석 
- 이전의 값을 재활용하는 규칙을 찾아서 점화식을 만들어서 푼다 (예: An = An-1 + An-2)

## ✅ 알고리즘
- 시간복잡도: O(N^2)
- 전부 탐색의 경우 O(N^2) 이지만 경로를 이진탐색처럼 생략하는 경우에는 O(NlogN) 까지 줄어들 수도 있다

### Bottom-Up
- dp 배열을 만들고 이전 인덱스들을 기준으로 점화식을 찾아 값을 넣어준다
- dp 배열은 오름차순이어야 한다
- 편의상 dp 배열의 1번부터 N번 인덱스를 사용한다. dp[0] 은 빈 값으로 둔다. (`dp[1]` ~ `dp[N]`)
- `dp[1]`, `dp[2]` 과 같이 초기값은 직접 세팅해주어야 하며, 만약 N 이 작을 경우에는 `ArrayIndexOutOfBoundsException` 이 발생할 수 있으므로 검증처리를 해준다

### Top-Down
- dp[N] 으로부터 재귀적으로 파라미터 N 을 낮춰가며 값을 찾아낸다
- dp[1] 과 같은 초기값은 미리 세팅해둔다
- 만약 해당 dp[n] 에 이미 값이 존재한다면 바로 return 을 해주어야 한다
- Top-Down(재귀함수)는 Bottom-Up(반복문)으로 변경할 수 있으며, 떠올리기 쉬운 방식대로 진행하면 된다

## ✅ 의사코드

### Bottom-Up (점화식)
```
private void recur() {
    if (N >= 1) {
        dp[1] = nums[1];
    }
    if (N >= 2) {
        dp[2] = nums[1] + nums[2];
    }
    if (N >= 3) {
        dp[3] = Math.max(dp[2], Math.max(nums[1] + nums[3], nums[2] + nums[3]));
    }

    for (int i=4; i<=N; i++) {
        int case1 = dp[i-1];
        int case2 = dp[i-2] + nums[i];
        int case3 = dp[i-3] + nums[i-1] + nums[i];
        dp[i] = Math.max(case1, Math.max(case2, case3));
    }
}
System.out.println(dp[N]);
```

#### Bottom-Up: 한 칸씩 dp[i] 값을 넣어주는 게 아니라, dp[i+x] 의 값을 넣어주는 경우
```
private int recur() {
    for (int date = 1; date <= N; date++) {
        if (date + times[date] > N+1) {
            continue;
        }
        dp[date + times[date]] = Math.max(dp[date + times[date]], dp[date] + prices[date]);
        dp[date + 1] = Math.max(dp[date + 1], dp[date]);
    }
    return dp[N+1];
}
```

### Top-Down

```
private int recur(int digit, int num) {
    if (digit == 1) {
        return dp[digit][num];
    }

    //만약 dp[digit][num] 에 이미 값이 들어있다면 바로 return 한다
    if (dp[digit][num] == 0) {
        if (num == 0) {
            dp[digit][num] = dfs(digit - 1, 1);
        } else if (num == 9) {
            dp[digit][num] = dfs(digit - 1, 8);
        } else {
            dp[digit][num] = dfs(digit - 1, num - 1) + dfs(digit - 1, num + 1);
        }
    }
    return dp[digit][num];
}
```

# ✅ LIS (Longest Increasing Sequence) 가장 긴 증가하는 부분 수열

## 문제분석
- 순서를 유지하면서, 특정 기준에 따라 가장 많은 원소를 선택해야 할 때
- 줄 세우기 문제: 최소한으로 움직여 정렬하고 싶을 때
- 전선 연결/다리 건설: 두 지점을 연결하는 선들이 서로 겹치지 않게 하면서 최대한 많이 연결하고 싶을 때

## 알고리즘
- 2중 for 문으로 돌면서 현재 인덱스를 기준으로 배열의 앞쪽의 부분수열을 비교하면서 가장 긴 부분수열을 뽑아낸다
- 시간복잡도: DP 와 동일하게 O(N^2)
- sequence = {10, 20, 10, 30, 20, 50}
- dp = {1, 2, 1, 3, 2, 4}
- dp[1] = {10}
- dp[2] = {10, 20}
- dp[3] = {10}
- dp[4] = {10, 20, 30}
- dp[5] = {10, 20}
- dp[6] = {10, 20, 30, 50}

## 의사코드

### Bottom-Up

```java
private int recur() {
    int[] dp = new int[N+1];
    
    for (int j=1; j<=N; j++) {
        dp[j] = 1;
        for (int i=1; i<j; i++) {
            if (A[i] < A[j] && dp[i] >= dp[j]) {
                dp[j] = dp[i] + 1;
            }
        }
    }
    
    return Arrays.stream(dp).max().orElse(-1);
}

```

### Top-Down

```java
private static int recur(int n) {
    if (dp[n] != 0) {
        return dp[n];
    }

    dp[n] = 1;
    for (int i=1; i<n; i++) {
        if (A[n] > A[i]) {
            dp[n] = Math.max(dp[n], recur(i) + 1);
        }
    }
    return dp[n];
}

for (int i=1; i<=N; i++) {
    recur(i);
}

int maxValue = 0;
for (int i=1; i<=N; i++) {
    maxValue = Math.max(maxValue, dp[i]);
}
System.out.println(maxValue);
```
