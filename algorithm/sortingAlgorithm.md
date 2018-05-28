# 검색알고리즘

1.  순차탐색 (Sequential Search) = 선형검색 (Linear Search)
	- 시간복잡도 : O(N)
	- 한 줄 정리 : 차례대로 인덱스를 높여가며 해당 수를 찾는 방법


2. 이분탐색 (Binary Search)
	- 시간복잡도 : O(NlogN)

3. 이진트리탐색 (Binary Tree Search)
	- 시간복잡도 : O(logN) , 만약 Tree가 한쪽으로만 치우쳐진 최악의 경우 O(N)

	* Red-Black Tree
		- 시간복잡도 : 최악의 경우여도 O(logN)

4. 해쉬 (Hashing)
	- 임의의 크기를 가진 데이터를 고정된 데이터의 크기로 변환시키는 것

	*  Direct Addressing Table
		- key-value쌍의 데이터를 배열에 저장할, Key값을 직접적으로 배열의 인덱스로 사용하는 방법

	* Hash Table
		- key값을 테이블에 저장할 때, direct Addressing Table과 달리 key값을 Hash Function을 이용해 계산을 수행한 후, 그 결과값을 배열의 인덱스로 사용하여 저장하는 방식. 

		* 문제점 

			-  충돌 (Collision)
				: 다른 key값임에도 h(k)값이 같아 충돌이 일어날 경우

			- Chaining방법
				: 다른 key값의 h(k)를 연결하여 value에는 h(k)들이 리스트 형태를 이룬다.

				* Simple Uniform Hash
				: 충돌이 적은 좋은 Hash Function을 만드는 방법.