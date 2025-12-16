# Disjoint-set (union find)

## ✅ 문제분석
- 합집합 연산을 해야하는 경우 

## ✅ 알고리즘
- parents 배열을 만들고 자기 부모 노드를 가리키게 하여 합집합을 만든다

## ✅ 의사코드

### find
```java
private int find(int x) {
    if (x == parents[x]) {
        return x;
    }
    return find(parents[x]);
}
```

### union
```java
private boolean union(int a, int b) {
    int parentA = find(a);
    int parentB = find(b);

    // 두 원소가 이미 같은 집합에 속해있기에 union 이 불가능하다
    if (parentA == parentB) {
        return false;
    }

    // 더 작은 값을 상위 노드로 설정한다 
    if (parentA < parentB) {
        parents[parentB] = parentA; 
    } else if (parentA > parentB) {
        parents[parentA] = parentB;
    }
    return true;
}
```
