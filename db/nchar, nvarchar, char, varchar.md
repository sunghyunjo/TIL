# nchar, nvarchar, char, varchar 차이

- nchar, nvarchar : 유니코드 저장 가능, 두 배의 저장 공간을 차지하므로, 유니코드 지원이 필요한 경우에만 사용하는 것이 좋음.
- char, varchar : 유니코드 문자 저장 불가
- char, nchar : 고정길이, 공간을 모두 사용하지 않아도 지정한 공간만큼 할당된다.
- varchar, nvarchar : 저장하는 문자의 공간만 사용하는 가변 길이, 사용하지 않는 공간은 할당되지 않는다.
