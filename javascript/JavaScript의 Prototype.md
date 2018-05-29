# JavaScript의 Prototype이해하기


-----

JavaScript는 프로토타입 기반 언어라고 불린다. JavaScript개발을 하면 빠질 수 없는 것이 프로토타입이라고 할 수 있을 정도로 중요한 개념이라고 할 수 있다. 
<br></br>

### Prototype vs Class
기존의 C#이나 JAVA같은 언어의 경우에는 Class에 작성한 정보를 바탕으로 인스턴스를 생성하여 객체를 사용한다. 그러나, JavaScript에는 Class가 존재하지 않았다. (ES6부터는 Class를 지원하기 시작하였다. 하지만, 문법이 추가되었다는 것이고 JavaScript가 Class기반으로 바뀐 것은 아니다.) 

그런데 중요한 점은 JavaScript도 객체지향언어라는 것이다. 객체지향언어로써 불릴 수 있는 가장 큰 이유는 Prototype이라는 것이 존재하기 때문이다. 

JavaScript에는 클래스가 없기 때문에 기본적으로 상속기능이 존재하지 않는다. 그래서 Prototype을 기반으로 상속을 흉내내도록 구현해 사용하게 된다. 해당 객체를 복사 또는 필요에 따라 하나하나 특성들을 확장해 나가는 방식으로써 Prototype을 이용하는 것이다.
<br></br>
### Prototype의 개념적인 비유
```
//프로그래머라는 존재가 있다.
var Programmer = function(){};

//프로그래머의 컴퓨터는 맥북이다. 
Programmer.prototype.computer = function(){
	console.log("I have a Macbook");
};

//그리고 프로그래머인 '성현', '현우'라는 존재가 생겨나기 시작한다.
var Sunghyun = new Programmer();
var Hyunwoo = new Programmer();

//성현과 현우 또한 프로그래머이기 때문에 그들의  컴퓨터도 맥북이다.
Sunghyun.computer();
> I have a Macbook
Hyunwoo.computer();
> I have a Macbook

//그런데 어느날 프로그래머의 컴퓨터가 삼성컴퓨터로 바뀝니다. 그렇다면, 이때 프로그래머인 '성현', '현우'의 컴퓨터들은 어떻게 될까요?
Programmer.prototype.computer = function(){
	console.log("I have a Samsung Laptop");
};

//'성현'과 '현우'의 노트북도 삼성으로 바뀌어 버렸습니다.
Sunghyun.computer();
> I have a Samsung Laptop
Hyunwoo.computer();
> I have a Samsung Laptop

//그런데 '성현'과 '현우'는 다시 맥북으로 돌아가고자 합니다.
Sunghyun.computer = function(){
	console.log("I have a Macbook");
}

Hyunwoo.computer = function(){
	console.log("I have a Macbook");
}

//다시 맥북으로 돌아왔습니다.
Sunghyun.computer();
> I have a Macbook
Hyunwoo.computer();
> I have a Macbook

```
대충 어떤 방식인지 이해가 갈 것이라고 생각한다. 또 다른 예제로 이해를 돕도록 하자.
<br>
아래의 예제는 JavaScript에는 Class는 없지만, 함수(function)와 new를 통해 클래스를 흉내내본 것이다.
```
function Person(){
	this.eyes = 2;
	this.nose = 1;
}

var kim = new Person();
var park = new Person();

console.log(kim.eyes); // => 2
console.log(kim.nose); // => 1

console.log(park.eyes); // => 2
console.log(park.nose); // => 1
```
kim과 park은 eyes와 nose를 공통적으로 가지고 있기 때문에, 메모리 속에는 eyes와 nose가 두 개씩 총 4개가 할당되게 된다. 그렇다면 객체를 n개 만들면 2n개의 변수가 메모리에 할당되게 될 것이다.
바로 이런 문제를 프로토타입으로 해결 할 수 있다.
```
function Person() {}

Person.prototype.eyes = 2;
Person.prototype.nose = 1;

var kim  = new Person();
var park = new Person():

console.log(kim.eyes); // => 2
...
```
간단히 설명하자면, Person.prototype이라는 빈Object가 어딘가에 존재하고, Person함수로부터 생성된 객체(kim, park)들은 어딘가에 존재하는 Object에 들어있는 값을 모두 갖다쓸 수 있다.
즉, eyes와 nose를 어딘가에 있는 빈 공간에 넣어놓고 kim과 park이 공유해서 사용하는 것이다.

### Prototype Object
Prototype에 대해 깊게 알아보기 전에, 먼저 Prototype Object가 생성되는 구조에 대해서 알아보자.
구조를 설명하기 위한 예제로는 맨 위에 작성한 '프로그래머의 컴퓨터 예제'를 사용하였다.

```
function Programmer(){};
console.dir(Programmer);
```
위의 코드를 실행하여 콘솔창을 살펴보면, 아래와 같다.
![](https://lh6.googleusercontent.com/2R7DSsqy49sDsbrP5n9b6_aWTzaxxFj_0xU6512QXEicJO8XSGGT12g7rdFe6wHxGKyoNHTX1piCa2A=w1920-h984)
우리가 여기서 봐야할 부분은 prototype 프로퍼티 이다. prototype객체의 constructor는 바로 원래의 함수인 Programmer를 바라보고 있다. 
즉, 함수를 생성하면 그 함수의 prototype객체가 만들어지고, 그 객체는 다시 원래의 함수를 생성자로서 바라보고 있다는 말이다.

![](https://cdn-images-1.medium.com/max/1600/1*PZe_YnLftVZwT1dNs1Iu0A.png)

즉, Hyunwoo객체와 Sunghyun객체는 Programmer prototype객체를 바라보게 된다. Programmer라는 함수 그 자체를 바라보는 것이 아닌 Programmer prototype객체를 참조해서 확장을 해나가는 것이다.
<br>

Object 객체에도 prototype의 개념은 역시나 적용되어있다. 우리가 일반적으로 많이 쓰는 일반적인 객체 생성에서 이를 찾아볼 수 있다.
```
var obj = {};
```
얼핏보면 이는 함수랑 전혀 상관없는 코드로 보이지만, 위 코드는 사실 다음 코드와 같다.
```
var obj = new Object();
```
위 코드에서 Object가 JavaScript에서 기본적으로 제공하는 함수다.
![](https://cdn-images-1.medium.com/max/1600/1*AJIDIoBFrGtUb8Nv-IonQg.png)
Object와 마찬가지로 Function, Array도 모두 함수로 정의되어 있다. 우리는 이를 사용할 때마다 해당 함수의 Prototype Object를 참조하는 것이다.

<br>
</br>
### Prototype Link & Prototype Chain
다시 맨 처음의 Programmer예제를 보자. Hyunwoo와 Sunghyun에게는 computer라는 속성이 없음에도 Hyunwoo.computer();를 실행하면 "I have a Macbook"이라는 값을 참조하는 것을 볼 수 있습니다. 위에서 설명했듯이 Prototype Object(=Programmer prototype)에 존재하는 computer속성을 참조한 것이다. 과연 이게 어떻게 가능한걸까?

바로 Hyunwoo나 Sunghyun이 가지고 있는` __proto__`가 그것을 가능하게 해주는 열쇠이다.
prototype속성은 함수만 가지고 있다는 것과는 달리(ex. Programmer.prototype) ` __proto__`속성은 모든 객체가 빠짐없이 가지고 있는 속성이다. 

` __proto__`는 객체가 생성될 때의 부모였던 함수의 Prototype Object를 가리킨다. Hyunwoo와 Sunghyun객체는 Programmer함수로부터 생성되었으니 Programmer함수의 Prototype Object를 가리키고 있는 것이다.

다시 두번째 예제로 돌아가서, 
![](https://cdn-images-1.medium.com/max/1600/1*jMTxqTYDZGhykJQoimmb0A.png)
kim객체가 eyes를 직접 가지고 있지 않기 때문에 eyes속성을 찾을 때까지 상위 Prototype을 탐색한다. 최상위 객체인 Object의 Prototype Object까지 도달했는데도 해당 속성을 못 찾았을 때에는 "undefined"를 리턴한다. 이렇게 상위 프로토타입과 연결되어있는 형태를 **Prototype Chain**이라고 한다.

이런 프로토타입 체인 구조로 인해서 모든 객체는 Object의 자식이라고 불리고, Object Propotype Object에 있는 모든 속성을 사용할 수 있다. 한 가지 예를 들자면 toString함수가 있다.
![](https://cdn-images-1.medium.com/max/1600/1*VW4PFea8x7LQiHp3PI8Hrg.png)


참고
[[Javascript ] 프로토타입 이해하기](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)
[[자바스크립트][OOP] prototype 이해하기 (개념)](http://yubylab.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
