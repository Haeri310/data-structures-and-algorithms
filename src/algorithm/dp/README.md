# ✅ DP (Bottom-Up)

## 문제분석 
- 이전의 값을 재활용하는 규칙을 찾아서 점화식을 만들어서 푼다 (예: An = An-1 + An-2)

## 알고리즘
- dp 배열을 만들고 이전 인덱스들을 기준으로 점화식을 찾아 값을 넣어준다
- dp 배열은 오름차순이어야 한다
- 편의상 dp 배열의 1번부터 N번 인덱스를 사용한다. dp[0] 은 빈 값으로 둔다. (`dp[1]` ~ `dp[N]`)
- `dp[1]`, `dp[2]` 과 같이 초기값은 직접 세팅해주어야 하며, 만약 N 이 작을 경우에는 `ArrayIndexOutOfBoundsException` 이 발생할 수 있으므로 검증처리를 해준다

## 의사코드

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

#### 한 칸씩 dp[i] 값을 넣어주는 게 아니라, dp[i+x] 의 값을 넣어주는 경우
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


# ✅ DP (Top-Down)

## 문제분석
- dp[1] 과 같은 초기값이 주어진다
- dp[N] 을 구하려면 이전의 값을 기준으로 구해야 한다 

## 알고리즘
- dp[N] 으로부터 재귀적으로 파라미터 N 을 낮춰가며 값을 찾아낸다
- dp[1] 과 같은 초기값은 미리 세팅해둔다
- 만약 해당 dp[n] 에 이미 값이 존재한다면 바로 return 을 해주어야 한다
- Top-Down(재귀함수)는 Bottom-Up(반복문)으로 변경할 수 있으며, 떠올리기 쉬운 방식대로 진행하면 된다

## 의사코드

```
private int dfs(int digit, int num) {
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
