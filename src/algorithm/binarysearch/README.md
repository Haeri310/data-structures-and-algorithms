# Binary-Search

## ✅ 문제분석
- 정렬이 가능한 리스트에서 빠르게 탐색을 하고 싶을 때 사용한다

## ✅ 알고리즘
- 정렬이 되어 있어야 한다
- A = [1, 2, 3, 4, 5] or [1, 2, 3, 4, 5, 6]
- left = 가장 왼쪽 인덱스, right = 가장 오른쪽 인덱스, mid = (left + right) / 2
- mid 와 target 을 비교한다
  - (mid == target) -> return mid;  
  - (mid < target) -> left = mid + 1
  - (target < mid) -> right = mid - 1
- left 가 right 보다 커지기까지 반복했는데 못 찾았으면 찾고자 하는 값은 없다 -> return -1;


## ✅ 의사코드

### 직접 구현
```
private int binarySearch(int target) {
    int left = 0;
    int right = numbers.size() - 1;
    int mid;
    while (left <= right) {
        mid = (left + right) / 2;
        if (numbers.get(mid) == target) {
            return mid;
        }
        if (numbers.get(mid) < target) {
            left = mid + 1;
        } else if (numbers.get(mid) > target) {
            right = mid - 1;
        }
    }
    return -1;
}
```


### 라이브러리 사용
```
int index = Collections.binarySearch(numbers, inputNumber);
```
- 인덱스 반환
- 만약 없다면 음수 반환 
