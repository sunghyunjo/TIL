# REST API에서 query string과 path param은 어떤 경우에 써야 할까?
아직 해당 문제에 대해 문서화하여 가이드라인을 제시하진 않는다.
정답은 아니니, 참고하는 형태로 사용하면 좋을 것이다.

## Query string을 쓰는 편이 나은 경우
- 매개 변수 값이 없는 경우, 404에러가 아닌 빈 리스트를 반환해야 하는 경우
ex. /contacts?name=dave 에서 dave에 해당하는 리스트가 없을 경우, 빈 리스트가 반환된다.
하지만, 만일 path param형태로 사용할 경우 해당하는 매개 변수 값이 없으면 404에러가 반환된다. ex. /customer/232
- filter나 sorting등의 optional한 조건이 필요한 경우

## Path param을 쓰는 편이 나은 경우
- 매개 변수가 URL의 전체 하위 로직에 영향을 미치는 경우
ex. /document/foo.txt?language=en 보다 /en/document/foo.txt가 낫다.

[참고](https://stackoverflow.com/questions/4024271/rest-api-best-practices-where-to-put-parameters)