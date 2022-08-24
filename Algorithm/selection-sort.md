## 선택 정렬
- 해당 순서에 원소를 넣을 위치는 이미 정해져 있고, 어떤 원소를 넣을지 선택하는 알고리즘이다.
- 배열에서 해당 자리를 선택하고 그 자리에 오는 값을 찾는 것이라고 생각하면 편하다.
- 제자리 정렬(in-place sorting)을 이용한다.

<br/>

### 제자리 정렬이란?
- 입력 배열 이외에는 다른 추가 메모리를 요구하지 않는 정렬 방법

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/selection-sort.png" width = 500/>

<br/>

### 선택 정렬 과정
1. 빈 리스트와 정렬되지 않은 숫자들이 있는 리스트를 준비한다. 
2. 정렬되지 않는 리스트에서 가장 작거나 큰 숫자를 추출한다. (기존 리스트에서 제거한다.)
3. 추출한 숫자를 빈 리스트에 차례로 삽입한다.
4. 정렬되지 않은 리스트가 공백이 되면 종료한다.

<br/>

### 선택 정렬의 특징
- 선택 정렬의 단점은 입력 배열과 크기가 같은 배열이 하나 더 필요하다는 것이다.
- 메모리를 절약하기 위해 추가 공간을 사용하지 않는 선택 정렬 알고리즘이 필요하다.
- 자료 이동 횟수가 미리 결정된다는 장점이 있다.
- 안정성을 만족하지 않는다는 단점이 있다.
    - 이는 값이 같은 레코드가 있는 경우 상대적인 위치가 변경될 수 있다는 의미이다.

<br/>

### 선택 정렬 구현
```java
public class Selection_Sort {
	public static void selection_sort(int[] a) {
		selection_sort(a, a.length);
	}

	private static void selection_sort(int[] a, int size) {
		for (int i = 0; i < size - 1; i++) {
			int min_index = i;

			//최솟값을 갖고 있는 인덱스 찾기
			for (int j = i + 1; j < size; j++) {
				if (a[j] < a[min_index]) {
					min_index = j;
						}
				}

			//i 번째 값과 찾은 최솟값을 서로 교환
			swap(a, min_index, i);
		}
	}

	private static void swap(int[] a, int i, int j) {
		int temp = a[i];
		a[i] = a[j];
		a[j] = temp;	
	}
}
```