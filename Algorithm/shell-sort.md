## 쉘 정렬
- 삽입 정렬이 어느 정도 정렬된 배열에 대해서는 대단히 빠른 것에 착안하여 보완한 알고리즘이다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/shell-sort.png" width = 500/>

<br/>

### 쉘 정렬의 과정
1. 정렬해야 할 리스트를 일정 기준에 따라 분류하여 연속적이지 않은 부분 리스트를 만든다.
2. 각 부분 리스트를 삽입 정렬을 이용하여 정렬한다.
3. 모든 부분 리스트가 정렬되면 다시 전체 리스트를 더 적은 개수로 이루어진 부분 리스트로 만든다.
4. 위 과정을 부분 리스트의 개수가 1이 될 때까지 되풀이 한다.

<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/shell-sort2.png" width = 500/>

<br/>

### 쉘 정렬의 원리
- 부분 리스트를 구성할 때는 주어진 리스트의 각 k(=간격, gap)번째 요소를 추출하여 만든다.
- 각 스텝(단계)마다 간격 k를 줄여가므로 수행 과정이 반복될 때마다 하나의 부분 리스트에 속하는 레코드들의 개수는 증가된다.
    - 결국 마지막 스텝에서는 간격 k의 값이 1이 된다.

- 실제로 부분 리스트들이 만들어지는 것이 아닌, 일정한 간격으로 삽입 정렬을 수행한 것 뿐이다.
    - 따라서 추가적인 공간은 필요하지 않다.

- 주로 스텝별로 간격을 절반으로 줄이는 방식을 많이 사용한다.

<br/>

### 쉘 정렬 구현
```java
public class Shell_Sort {
	private final static int[] gap = 
		{ 1, 4, 10, 23, 57, 132, 301, 701, 1750, 3937, 	
		8858, 19930, 44842, 100894, 227011, 510774,
		1149241, 2585792, 5818032, 13090572, 29453787, 
		66271020, 149109795, 335497038, 754868335, 1698453753};	
        // 갭을 담고있는 배열	
		 
			
	public static void shell_sort(int[] a) {
		shell_sort(a, a.length);
	}
 
	// 맨 처음 gap을 참조 할 인덱스를 구하는 메소드
	private static int getGap(int length) {
		int index = 0;
		// 최소한 부분 배열의 원소가 2개씩은 비교 되도록 나눠준다.
		int len = (int)(length / 2.25);	
				
		while (gap[index] < len) {
			index++;
		}
		return index;
	}

	private static void shell_sort(int[] a, int size) {
	    int gapIndex = getGap(size);
				
		// 갭이 1이 될 때까지 반복
		while(gapIndex >= 0) {
			int step = gap[gapIndex--];	// 현재 gap(step)
						
			/*
			 * --- 삽입 정렬 과정 ---
			 * 
			 * 각 부분리스트의 두 번째 원소의 인덱스 부터 순회한다.
			 * 예로들어 step이 3일 때 arr[0], arr[1], arr[2] 는 
			 * 이전 원소와 비교할 것이 없다.
			 * 그러므로 step부터 순회한다.   
			 */
			for(int i = step; i < size; i++) {
							
			/*
			 *  j는 target 원소가 되며 현재 원소(target) a[j]가 
			 *  이전 원소 a[j - step]보다 작을 때 까지 반복한다.
			 */
				for (int j = i; j >= step &&
                 a[j] < a[j - step]; j -= step) {
			/*
    		 *  현재(target) 원소의 인덱스(j)와 
			 *  이전의 원소(j-step)의 인덱스에 있는 원소의 값을 교환한다.
			 */
					swap(a, j, j - step);
				}
			}
		}
	}

		private static void swap(int[] a, int i, int j) {
				int swap = a[i];
				a[i] = a[j];
				a[j] = swap;
		}
}
```

<br/>

### 쉘 정렬의 복잡도 분석
- 연속적이지 않은 부분 리스트에서 자료의 교환이 일어나면 더 큰 거리를 이동한다.
    - 반면 삽입 정렬에서는 한 번에 한 칸씩만 이동된다.
    - 따라서 교환되는 아이템들이 삽입 정렬보다는 최종 위치에 더 가까이 있을 가능성이 높다.

- 시간 복잡도는 최악의 경우 $O(n^2)$이지만, 평균적인 경우 O(n^1.5)로 나타난다.

<br/>

### 쉘 정렬의 특징
- 삽입 정렬의 시간 복잡도 $O(n^2)$보다 빠르다.
- 멀리 떨어진 위치로도 이동할 수 있다.
- 삽입 정렬과는 다르게 쉘 정렬은 전체의 리스트를 한 번에 정렬하지 않는다.
- 최대 문제점은 요소들이 삽입될 때, 이웃한 위치로만 이동한다는 것이다.
    - 만약 삽입되어야 할 위치가 멀리 떨어진 곳이라면 많은 이동을 해야 한다.