# ready와 load이벤트 차이
우리가 평소에 javascript파일을 구동할 때, 사용하는 jQuery함수인 ready와 load는 어떤 차이가 있을까?

## $(document).ready()
브라우저에서 DOM트리를 생성하고난 후에 실행되게 하는 코드이다. 
DOM이 준비가 다 되었다는 뜻.
- ready는 document객체에 한해서만 적용되는 메소드이다.
- window.load()보다 더 빠르게 실행되고, 중복 사용하여 실행해도 선언한 순서대로 실행된다.

## $(window).load()
include되어 있는 모든 object, image, frame등이 모두 로드된 후에 실행된다.
- load는 모든 외부자원(css, js, images, scripts, frames, iframes 등)과 windwo객체에 적용되는 메소드이다.
- 전체 페이지의 모든 외부 리소스나 이미지를 브라우저에 부른 후 작동하게 되기 때문에, 이미지가 안뜨는 딜레이가 생기면 그만큼의 딜레이가 생긴다.
- window는 document의 부모객체로 븝라우저 자체를 의미할 수 있고, 접근할 수 있는 자식객체로는 document, self, navigator, screen, forms, history, location등이 있다.

## 정리하자면..
외부자원이나 window객체가 모두 로드되었을 때에 그 자원들을 가공해아 한다면 load이벤트를 쓰고, 브라우저에서 보이게 될 html코드에 대해서만 가공할 필요가 있다면 ready를 사용하는게 더 낫다.



[참고문서1](http://secr.tistory.com/111), [참고문서2](http://diaryofgreen.tistory.com/96)