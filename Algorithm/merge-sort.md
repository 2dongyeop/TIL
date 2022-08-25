## 합병 정렬

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/merge-sort.png" width = 500/>

<br/>

### 합병 정렬 원리
- 합병 정렬은 분할 정복 기법에 바탕을 두고 있다.
    - 분할 정복이란?
        - 2개의 문제로 분리하여 각각 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 전략이다.
        - 분리된 문제가 아직 해결되지 않았다면, 분할 정복 방법을 연속하여 다시 적용한다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/merge-sort2.png" width = 500/>

### 합병 정렬 과정
1. 하나의 리스트를 두 개의 균등한 크기로 분할한다.
2. 분할된 부분 리스트를 정렬한다.
3. 두 개의 정렬된 부분 리스트를 합한다.
4. 리스트를 정렬한다.

<br/>

### 합병 정렬 구현
```java
public class Merge_Sort {
	private static int[] sorted;

	public static void merge_sort(int[] a) {
		sorted = new int[a.length];
		merge_sort(a, 0, a.length - 1);
		sorted = null;
	}

	private static void merge_sort(int[] a, int left, int right) {
		//left == right 즉, 부분 리스트가 1개의 원소만 갖고 있는 경우
		//더 이상 조갤 수 없으므로 return한다.

		if (left == right) return;
			
		int mid = (left + right) / 2;

		merge_sort(a, left, mid);  //절반 중 왼쪽 부분리스트
		merge_sort(a, mid + 1, right);

		merge(a, left, mid, right);
	}

	private static void merge(int[] a, int left, int mid, int right) {
		int l = left;
		int r = mid + 1;
		int idx = left;

		while(l <= mid && r <= right) {
			if (a[l] <= a[r]) {
				sorted[idx] = a[l];
				idx++;
				l++;
			} else {
				sorted[idx] = a[r];
				idx++;
				r++;
			}
		}

		if (l > mid) {
			while (r <= right) {
				sorted[idx] = a[r];
				idx++;
				r++;
			}
		} else {
			while (l <= mid) {
				sorted[idx] = a[l];
				idx++;
				l++;
			}
		}

		for (int i = left; i <= right; i++) {
			a[i] = sorted[i];
		}
	}
}
```

<br/>

### 합병 정렬 복잡도 분석
- 합병 정렬은 비교 연산과 이동 연산의 경우 $O(nlog_2n)$의 복잡도를 가진다.

<br/>

### 합병 정렬의 장점
- 안정적인 정렬 방법이며, 데이터의 분포에 영향을 덜 받는다.
    - 즉, 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다.
    - 그러므로 최악, 평균, 최선의 경우가 다 같이 $O(nlog_2n)$인 정렬 방법이다.

<br/>

### 합병 정렬의 단점
- 임시 배열이 필요하다.
- 만약 레코드들의 크기가 큰 경우, 이동 횟수가 많다.
    - 하지만 레코드를 연결 리스트로 구성할 경우, 합병 정렬에서 데이터의 이동 횟수가 작아진다.
    - 따라서 크기가 큰 레코드를 정렬할 경우, 합병 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법보다 더 효율적이다.