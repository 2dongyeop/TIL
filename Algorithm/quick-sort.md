## 퀵 정렬
- 퀵 정렬(quick sort)은 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법이다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/quick-sort.png" width = 500/>

<br/>

### 퀵 정렬 원리
- 퀵 정렬도 [합병 정렬](https://github.com/2dongyeop/TIL/blob/main/Algorithm/merge-sort.md)과 동일하게 분할 정복에 근거한다.
- 합병 정렬과 비슷하게 전체 리스트를 2개의 부분 리스트로 분할하고,
각각의 부분 리스트를 다시 퀵 정렬하는 전형적인 분할 - 정복 방법을 사용한다.
- 하지만 합병 정렬과는 달리 퀵 정렬은 리스트를 비균등하게 분할한다.

<br/>

### 퀵 정렬 과정
1. 리스트 안에 있는 한 요소를 피벗(pivot)으로 선택한다.
2. 피벗보다 작은 요소들은 모두 피벗의 왼쪽으로 옮겨지고,
  피벗보다 큰 요소들은 모두 피벗의 오른쪽으로 옮겨진다.

결과적으로 피벗을 중심으로 왼쪽은 작은 요소들로, 오른쪽은 큰 요소들로 구성된다.

3. 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬하게 되면 전체 리스트가 정렬된다.

<br/>

### 퀵 정렬 구현
```java
public static void quickSort(final int[] list, final int start, final int end) {
    if (start < end) {
        final int pivot = partition(list, start, end);
        quickSort(list, start, pivot - 1);
        quickSort(list, pivot + 1, end);
    }
}

private static int partition(final int[] list, final int start, final int end) {
    int low, high;
    int pivot;

    low = start;
    high = end + 1;
    pivot = list[start];

    do {
        do {
            low++;
        } while (list[low] < pivot && low < end);

        do {
            high--;
        } while (list[high] > pivot && high > start);
        if (low < high) {
            SwapArrayUtils.swap(list, low, high);
        }
    } while (low < high);

    SwapArrayUtils.swap(list, start, high);

    return high;
}
```

<br/>

### 퀵 정렬 복잡도 분석

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/quick-sort2.png" width = 500/>

<br/>

- n이 2의 거듭제곱이라고 가정한다면, n개의 레코드를 가지는 리스트는 n/2, n/4, n/8 …의 크기로 나뉜다.
    - 크기가 1이 될 때까지 나누어지므로 $k=log_2n$개의 패스가 필요하게 된다.
- 각각의 패스에서는 평균 n번 정도의 비교가 이루어지므로 $O(nlog_2n)$의 복잡도를 가진다.

- 퀵 정렬은 최악의 경우, 리스트가 계속 불균형하게 나누어지는 것이다.
    - 이는 이미 정렬된 리스트에 대하여 퀵 정렬을 실행할 경우에 특히 많이 발생한다.
    - 최악의 경우 $O(n^2)$의 시간 복잡도를 가진다.

- 퀵 정렬은 속도가 빠르고 추가 메모리 공간을 필요로 하지 않는 장점이 있지만,
정렬된 리스트에 대해서는 오히려 수행 시간이 더 많이 걸리기도 한다.
- 따라서 이러한 불균형 분할을 방지하기 위해 피벗을 리스트의 중앙 부분을 선택해야 한다.