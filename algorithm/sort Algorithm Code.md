# Bubble Sort
- Time Complexity: O(n^2)

```
function bubbleSort(data) {
	for (var i = 0; i < data.length; i++) {
		for (var j = 0; j < data.length-1-i; j++) {
			if (data[j] > data[j+1]) {
				var temp = data[j+1];
				data[j+1] = data[j];
				data[j] = temp;
			}
		}
	}
}
```

# Insertion Sort
- Time Complexity: O(n^2)

```
function insertionSort(data) {
	for (var i = 1; i < data.length; i++) {
		var temp = data[i];
		for (var j = i-1; j >= 0 && temp < data[j]; j--) {
			data[j+1] = data[j];
		}
		data[j+1] = temp;
	}
}
```

# Selection Sort
- Time Complexity: O(n^2)

```
function selectionSort(data) {
	for (var i = 0; i < data.length; i++) {
		minIdx = i;
		for (var j = i+1; j < data.length; j++) {
			if (data[j] < data[minIdx]) {
				minIdx = j;
			}
		}
		var temp = data[minIdx];
		data[minIdx] = data[i];
		data[i] = temp;
	}
}
```

# Quick Sort
- Time Complexity : O(nlogn)
- 장점 : 가장 빠르다.
- 단점 : 피벗 값이 계속 최대값이나 최소값으로 잡는 경우 최악의 성능O(n^2)을 가짐.

```
function quickSort(data) {
	if (data.length === 0)
		return [];

	var middle = data[0];
	var len = data.length;

	var left = [];
	var right = [];

	for (var i = 1; i < data.length; i++) {
		if (arr[i] < middle)
			left.push(data[i]);
		else
			right.push(data[i]);
	}

	return quickSort(left).concat(middle, quickSort(right));
}

```


# Merge Sort
- Time Complexity : O(nlogn)
- 장점 : 데이터 상태에 크게 영향받지 않음
- 단점 : 데이터 크기만큼 메모리가 더 필요함

# Heap Sort
- Time Complexity : O(nlogn)
- 장점 : 추가적인 메모리가 필요하지 않음. 항상 O(nlogn)의 성능을 유지
- 단점 : Quick Sort보다 느림

