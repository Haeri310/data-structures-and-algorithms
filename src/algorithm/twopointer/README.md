# 투포인터

## 문제분석
1. 정렬이 가능한 리스트이며
2. 특정 조건을 만족하는 쌍(Pair)이나 부분배열(SubArray)을 찾아야 할 때
3. 시간복잡도 O(N)로 순회하며 풀이 가능성이 높을 때
4. 제자리에서 데이터 수정이나 중복 제거가 필요할 때 (하나는 Reader 다른 하나는 Writer 로 정렬된 배열에서 중복 제거)

## 알고리즘
1. 포인터 두 개를 설정 (주로 하나는 맨 앞, 다른 하나는 맨 뒤 / 둘 다 맨 앞)
2. 두 포인터를 움직이며 조건을 맞는 경우를 찾는다
3. 시간복잡도 O(N)

## 의사코드

```
private boolean twoPointer(int target) {
    int left = 0;
    int right = list.size() - 1;

    while (left < right) {
        int sum = list.get(left) + list.get(right);
        if (sum == target) {
            return true;
        }

        if (sum < target) {
            left++;
        } else if (sum > target) {
            right--;
        }
    }
    return false;
}
```

### 리스트에 중복이 있는 경우
- 조건을 만족하는 경우, 동일한 숫자는 left right 포인터를 옮기면서 count 하여 곱해준다

```
private void twoPointer(int target) {
    int left = 0;
    int right = N - 1;

    while (left < right) {
        int leftValue = nums.get(left);
        int rightValue = nums.get(right);

        int sum = leftValue + rightValue;
        if (sum == target) {

            //만약 left와 right가 같은 값을 가리키는 경우 (예: [5, 5])
            if (leftValue == rightValue) {
                int n = right - left + 1;
                count += n * (n-1) / 2; //조합 nC2
                break;
            }

            //left의 중복 세기
            int leftCount = 0;
            while (leftValue == nums.get(left)) {
                left++;
                leftCount++;
            }

            //right의 중복 세기
            int rightCount = 0;
            while (rightValue == nums.get(right)) {
                right--;
                rightCount++;
            }

            count += leftCount * rightCount;
        } else if (sum < target) {
            left++;
        } else if (sum > target) {
            right--;
        }
    }
}
```

### 투포인터로 검사하는 범위를 순회하는 경우
- left 포인터의 시작을 순회하는 인덱스 + 1 부터 시작한다

```
private boolean twoPointer(int index, int target) {
    int left = index + 1;
    int right = list.size() - 1;

    while (left < right) {
        int sum = list.get(left) + list.get(right);
        if (sum == target) {
            return true;
        }

        if (sum < target) {
            left++;
        } else if (sum > target) {
            right--;
        }
    }
    return false;
}
```
