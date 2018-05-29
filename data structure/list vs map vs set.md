## List (LinkedList, Vector, ArrayList).
- 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.
- Index를 사용하여 특정 위치에 요소를 삽입하거나 접근할 수 있으며 중복요소를 허용한다.

##	Vector
- 멀티쓰레드 환경에서도 사용될 수 있도록 객체의 삽입, 삭제 시에 동기화를 보장.
- 그러나 멀티쓰레드 환경이 아니면 ArrayList보다 성능이 떨어짐.

## ArrayList
- Add메소드의 실행이 가장 빠르고 배열의 크기를 자유자재로 늘릴 수 있음. 
- 하지만, Vector처럼 동기화 과정을 제공하지 않음.

##	LinkedList
- 스택, 큐 등 사용자가 ADT를 구축할 대 내부에서 사용되기 위한 측면이 강함.
- 단순히, 객체에 대한 참조를 넣고 배기 위해서라면 Vector나 ArrayList를 쓰는 것이 바람직

## Set (HashSet, LinkedHashSet)
- List가 항목의 순서를 관리하는 데에 비해 Set은 중복된 항목을 방지하는 데에 관심이 많음.

## HashSet
- 가장 빠른 접근을 보장함.
- 내부적으로 HashMap을 이용함. 
- HashSet에 추가되는 항목은 HashMap의 키값으로 저장되기 때문에, HashSet은 HashMap과 사용자 사이의 중계역할만을 담당.
- HashSet에서는 항목의 순서를 확인할 수 없음.
- 새로운 객체가 HashSet에 들어오면 내부에서 객체의 참조 위치가 바뀔 수도 있는데, 이는 HashSet내부에서 이용하는 HashMap의 특성때문.

## LinkedHashSet
- Set에 순서를 부여하기 위해 등장.
- 내부에서 double-linked list를 구현하여 원소가 추가된 순서를 유지함.

## Map
- 하나의 Map 내에서 같은 값을 가지는 key는 존재할 수 없음.
- 수집된 순서를 기억하지 않음.
