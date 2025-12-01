# DP

## 문제분석 
- 이전의 값을 재활용하는 규칙을 찾아서 점화식을 만들어서 푼다 (예: An = An-1 + An-2)

## 알고리즘
- dp 배열을 만들고 이전 인덱스들을 기준으로 점화식을 찾아 값을 넣어준다
- dp 배열은 오름차순이어야 한다
- 편의상 dp 배열의 1번부터 N번 인덱스를 사용한다. dp[0] 은 빈 값으로 둔다. (`dp[1]` ~ `dp[N]`)
- `dp[1]`, `dp[2]` 과 같이 초기값은 직접 세팅해주어야 하며, 만약 N 이 작을 경우에는 `ArrayIndexOutOfBoundsException` 이 발생할 수 있으므로 검증처리를 해준다

## 의사코드

```
private void dp() {
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

 

