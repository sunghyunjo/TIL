# Heap vs Stack
-	운영체제는 우리가 실행시킨 프로그램을 위해 프로그램이 실행될 때 마다 메인 메모리인 RAM에 메모리 공간을 할당해주게 된다.

## Data영역 
- 전역변수와 static변수가 할당되는 영역
- 프로그램의 시작과 동시에 할당되고, 프로그램이 종료되어야 메모리에서 소멸됨.

## Stack영역
- 함수 호출 시 생성되는 지역변수와 매개 변수가 저장되는 영역
- 함수 호출이 완료되면 사라짐.

## Heap영역
- 필요에 의해 동적으로 메모리를 할당 할 때 사용
- 할당해야 할 메모리의 크기를 프로그램이 실행되는 동안 결정해야 할는 경우(런타임 때) 유용하게 사용되는 공간.
- 배열이나, 클래스 , 인터페이스 등의 데이터 타입.
- 사용하지 않을 때는 처리해줘야함. 가비지 컬렉터가 그 역할을 대신하기도 함.