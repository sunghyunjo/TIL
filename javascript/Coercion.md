# 값 변환

## 명시적 강제변환 (Implicit Coercion)
- 의도적인 타입변환

```js
var a = 42;
var c = String(a);
```

### 문자열 <--> 숫자
- `String()`, `Number()` 사용
- +/- 단항연산자를 다른 연산자와 인접하여 사용하지 않는 것을 추천한다.

> `1 + - + + + - + 1; //2` 와 같은 가독성 없는 무수한 조합이 나올 가능성이 있다.

### 날짜 <--> 숫자
- + 단항연산자는 'Date 객체 -> 숫자' 강제변환 용도로도 쓰인다.
- 결괏값이 날짜/시각 값을 유닉스 타임스탬프 표현형 (1 January 1970 00:00:00 UTC 이후로 경과한 시간을 밀리 초 단위로 표시)이기 때문이다.

### 틸드(~)
- 숫자 값에 | 나 ~ 비트연산자를 적용하면 전혀 다른 숫자 값을 생성하는 강제변환 효과가 있다.
- 예를 들어, 아무 연산도 하지 않는 0 | x 의 'OR'연산자(|)는 사실상 ToInt32변환만 수행한다.

```
0 | -0; // 0
0 | NaN; // 0
0 | Infinity; // 0
0 | -Infinity; // 0
```

- 위와 같은 특수 숫자들은 32비트로 나타내는 것이 불가능하므로 ToInt32 연산 결과는 0이다.
- ~ 연산자는 먼저 32비트 숫자로 '강제변환'한 후, NOT연산을 한다. (각 비트를 거꾸로 뒤집는다.)

### 숫자 형태의 문자열 파싱
- 문자열로부터 숫자 값의 파싱은 비 숫자형 문자 (Non-Numeric Character)를 허용한다.

	> 즉, 좌 -> 우 방향으로 파싱하다가 숫자 같지 않은 문자를 만나면 즉시 멈춘다. 반면, 강제변환은 비 숫자형 문자를 허용하지 않기 대문에 NaN을 내보내고 더이상 아무 동작을 하지 않는다.

- 파싱은 강제 변환의 대안이 될 수 없다.
- 우측에 비 숫자형 문자가 있을지 확실하지 않거나 별로 상관없다면 문자열을 숫자로 파싱한다. 반드시 숫자여야만 하고 '42px'같은 값은 되돌려야 한다면 문자열을 숫자로 강제변환한다.	
### *(Non-Boolean) -> Boolean
- `Boolean()`은 명시적인 강제변환이다. 
- 일반적으로 자바스크립트 개발 시, 불리언 값으로 변환 시 truthy, falsy값까지 뒤바뀔 수 있기 때문에 이중 부정 연산자를 사용한다. 두번째 !부정연산자가 해당 패리티를 다시 원상 복구하기 때문이다.

## 암시적 강제변환 (Explicit Coercion)
- 다른 작업 도중 side effect로부터 발생하는 타입변환

```js
var a = 42;
var b = a + "";
```

## 강제변환의 나쁜 부분 7가지

```js
"0" == false; // true 
flase == 0; // true
false == ""; //true
false == []; //true
"" == 0; // true
"" == []; // true
0 == []; // true
```

## 암시적 강제변환의 안전한 사용법
- 피연산자 중 하나가 true/false일 가능성이 있으면 '절대로' == 연산자를 쓰지 말자.
- 피연산자 중 하나가 [], " ", 0이 될 가능성이 있으면 가급적 == 연산자는 쓰지 말자.

# 문법
## 콘텍스트 규칙
### 중괄호 {}
자바스ㅡ립트에서 중괄호가 나올만한 곳은 2가지 이다.
- 객체 리터럴
- 레이블

#### 객체리터럴
```js
// bar() 함수는 앞에서 정의되었다.

var a = {
	foo: bar()
};	
```

{ }는 a에 할당될 값이므로 객체 리터럴이 맞다.

#### 레이블
위의 객체리터럴 코드에서 var a = 부분을 삭제한다면, {}는 어디에도 할당되지 않은 객체 리터럴처럼 보일 수 있다.
하지만, 여기서의 { }는 평범한 코드 블록이다. 

```
// bar() 함수는 앞에서 정의되었다.

{
	foo: bar()
}	
```

이 { }코드 블록은 for/while루프, if조건 등에 붙어있는 코드 블록과 기능적으로 매우 유사하다.

위와 같은 구문은 자바스크립트에서 **레이블 문(Labeled Statement)**이라 부르는 기능이다.

이는 goto문과 비슷한 기능을 한다.

> 레이블 루프 블록은 사용 빈도가 극히 드물고 안좋은 구석이 많아 가능한한 사용하지 않는 편이 낫다.


#### try...finally
- finally절의 코드는 반드시 실행되고, 다른 코드로 넘어가기 전에 try이후부터 (catch가 있으면 catch 다음부터) 항상 실행된다.
- finally절의 return은 그 이전에 실행된 try나 catch절의 return을 덮어쓴다. 

[출처 : You don't know JS]