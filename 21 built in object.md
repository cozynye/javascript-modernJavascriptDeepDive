# 빌트인 객체

## 1. 자바스크립트 객체의 분류

1. **표준 빌트인 객체** - ECMAScript 사양에 정의된 객체, 애플리케잉션 전역의 공통 기능을 제공
2. **호스트 객체** - ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행환경에서 추가로 제공하는 객체
3. **사용자 정의 객체** - 사용자가 직접 정의한 객체



<br>

<br>

<br>

## 2. 표준 빌트인 객체

자바스크립트는 Object, String, Number, Boolean, Symbol, Math, Reflect, JSON, Proxy 등 40여 개의 표준 빌트인 객체를 제공한다.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.

생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공,

생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공

<br>

생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다
Object.getPrototypeOf(strObj) === String.prototype // true
```

<br>

표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공.

표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공.

```js
// Number.porototype은 다양한 기능의 빌트인 프로토타입 메서드를 제공

// Number 생성자 함수에 의한 Number 객체 생성
const numberObj = new Number(1.5)

// toFixed는 Number.prototype의 프로토타입 메서드
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환
numberObj.toFixed(); //2

// isInteger는 Number의 정적 메서드
// Number.isInteger는 인수가 정수인지 검사하여 그 결과를 Boolean으로 반환
Number.isInteger(0.5); // false
```



<br>

<br>

<br>

## 3. 원시값과 래퍼 객체

원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 

생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

```js
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작
str.length; // 5
str.toUpperCase(); // HELLO
```

위처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 **래퍼 객체**(wrapper object)라 한다.

```js
// 문자열에 대해 마침표 표기법으로 접근하면 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고
// 문자열은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.

const str = 'hello';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환
str.length; // 5
str.toUpperCase(); // HELLO

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
typeof str // string
```

<br>

이때 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상송받아 사용할 수 있다.

<img src="./image/built in object.png" height="600">

그 후 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 원래의 상태,

즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

<br>

```js
// 식별자 str은 문자열을 값으로 가지고 있다.
const str = 'hello';

// 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체의 name 프로퍼티가 동적 추가된다.
str.name = 'Lee';

// 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다
// 암묵적으로 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다

// 식별자 str은 새롭게 암묵적으로 생성된(위의 래퍼객체와는 다른) 래퍼 객체를 가리킨다
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
str.name // undefined;

// 식별자 str은 다시 원래의 문자열, 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다
// 새롭게 암묵적으로 생성된 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다
typeof str //string
```



<br><br><br>

## 4. 전역 객체

**전역 객체**(global object)는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며,

어떤 객체에도 속하지 않은 최상위 객체다.

브라우저 환경에서는 window(또는 self,this,frames)가 전역 객체이지만 Node.js 환경에서는 global이 전역 객체를 가리킨다.

<br>

전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체(클라이언트 Web API 또는 Node.js의 호스트 API), 

그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

<br>

전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체.

전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 

호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

<br>

전역 객체의 특징은 다음과 같다.

1. 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
2. 전역 객체의 프로퍼티를 참조할 때 window(global)를 생략할 수 있다.

```js
// 문자열 'F'를 16진수로 해석하여 10진수로 변환하여 반환
window.parseInt('F', 16) // 15
parseInt('F', 16) // 15

window.parseInt === parseInt // true
```

3. 전역 객체는 Object, String, Number, Fucntion, Promise 등과 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
4. 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다. 브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpReqeust 같은 클라이언트 사이드 Web API를 호스트 객체로 제공하고, Node.js 환경에서는 고유의 API를 호스트 객체로 제공한다.
5. var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.

```js
var foo = 1;
window.foo // 1
```

6. let이나 const 키워드로 선언한 변수는 전역 객체의 프로퍼티가 아니다.
7. 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다. 여러 개의 script 태그를 통해 자바스크립트 코드를 분리해도 하나의 전역 객체 window를 공유하는 것은 변함없다.

<br><br>

### 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미하며 주로 애플리케이션 전역에서 사용하는 값을 제공한다.



<br>

**Infinity**

Infinity 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.

```js
window.Infinity === Infinity // true
3/0 // Infinity
-3/0 // -Infinity
typeof Infinity // number
```



<br>

**NaN**

NaN 프로퍼티는 숫자가 아님(Not a Number)을 나타내는 숫자값 NaN을 갖는다. NaN 프로퍼티는 Number.NaN 프로퍼티와 같다.

```js
window.NaN // NaN
Number('xxx'); // NaN
1 * 'string' // NaN
typeof NaN // number
```



<br>

**undefined**

undefined 프로퍼티는 원시 타입 undefined를 값으로 갖는다.

```js
window.undefined // undefined

var foo;
foo // undefined
typeof undefined // undefined
```



<br>

<br>

### 빌트인 전역 함수

빌트인 전역 함수(built-in global function)는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.



<br>

**eval**

eval 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달 받고 전달받은 문자열 코드가 표현식이라면 eval 함수는

문자열 코드를 런타임에 평가하여 값을 생성, 표현식이 아니라면 eval 함수는 문자열 코드를 런타임에 실행.

문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행.

```js
// 표현식인 문
eval('1 + 2;'); // 3
// 표현식이 아닌 문
eval('var x = 5'); // undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언됨.
console.log(x); // 5

// 객체 리터럴, 함수 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('({ a : 1})');
const f = eval('(function(){return 1;})')
console.log(o); // {a:1}
console.log(f()); // 1

// 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행한 다음, 마지막 결과값을 반환
eval('1 + 2; 3 + 4;'); // 7
```

eval 함수는 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정한다.

```js
const x = 1;

function foo(){
    eval('var x = 2;');
    console.log(x); // 2
    
    // strict mode에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프를 생성한다.
    'use strict';
    eval('var x = 5; console.log(x)'); // 5
    
    console.log(x) // 2
}
foo(); // 2
console.log(x); // 1


function foo2(){
    // strict mode에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프를 생성한다.
    'use strict';
    eval('var x = 5; console.log(x)'); // 5
    
    console.log(x) // 1
}
foo2(); 

function foo3(){
    eval('var x = 5; console.log(x)'); // 5
    
    // let, const 키워드를 사용한 변수 선언문은 strict mode가 적용된다.
    eval('const x = 7; console.log(x)'); // 7
    
    console.log(x) // 5
}
foo3(); 
```

eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 취약하고 최적화가 수행되지 않으므로 일반적인 코드 실행에

비래 처리 속도가 느리므로 쓰지 않는 것이 좋다.



<br>

**isFinite**

전달받은 인수가 유한수이면 true를 반환, 무한수이면 false를 반환.

인수의 타입이 숫자가 아닌 경우, 숫자로 타입을 변환한 후 검사를 수행하며 NaN으로 평가되는 값이면 false를 반환.

```js
isFinite(null); // true why? null ->0

isFinite(Infinity); // false
isFinite(-Infinity); // false

isFinite(NaN); // false
isFinite('Hello'); // false
isFinite(-'2005/12'); // false
```



<br>

**isNaN**

전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환.

인수의 타입이 숫자가 아닌 경우 숫자로 타입을 변환한 후 검사를 수행.

```js
isNaN(NaN) // true
isNaN(10) // false

isNaN('hello') // true why? 'hello' => NaN
isNaN('10.20') // false
isNaN('') // false why? ''->0

isNaN(true); // false
isNaN(false); // false

isNaN(undefined) // true why? undefined -> NaN

isNaN({}); // true why? {} -> NaN

isNaN(new Date()); // false why? new Date() -> Number
isNaN(new Date().toString()) // true why? string is non number
```



<br>

**parseFloat**

전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환

```js
parseFloat('3.14') // 3.14
parseFloat('10.00') // 10

parseFloat('34 55 66') // 34
parseFloat('30 year') // 30

parseFloat('hello') // NaN

parseFloat('    60    ') // 60
```



<br>

**parseInt**

전달받은 문자열 인수를 정수로 해석하여 반환.

```js
parseInt('10') // 10
parseInt('10.223')  // 10

parseInt(10) // 10
parseInt(10.223)  // 10
```

두 번째 인수로 진법을 나타내는 기수(2 ~ 36)를 전달.

기수를 지정하면 첫 번째 인수로 전달된 문자열을 해당 기수의 숫자로 해석하여 반환. 반환값은 언제나 10진수이다.

기수를 생략하면 10진수로 해석하여 반환.

```js
// '10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환
parseInt('10'); // 10
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환
parseInt('10', 8); // 8

// 첫 번째 인수로 전달한 문자열의 첫 번째 문자가 해당 지수의 숫자로 변환될 수 없다면 NaN을 반환
// 'A'는 10진수로 해석할 수 없다.
parseInt('A0') // NaN
// '2'는 2진수로 해석할 수 없다.
parseInt('20', 2 ) // NaN

// 첫 번째 인수로 전달한 문자열의 두 번째 문자부터 해당 진수를 나타내는 숫자가 아닌 문자와 마주치면
// 이 문자와 계속되는 문자들은 전부 무시되며 해석된 정수값만 반환.
// 10진수로 해석할 수 없는 'A' 이후의 문자는 모두 무시
parseInt('3A0') // 3
// 8진수로 해석할 수 없는 '8' 이후의 문자는 모두 무시
parseInt('789', 8) // 7

// 첫번째 인수로 전달한 문자열에 공백이 있다면 첫 번째 문자열만 해석하여 반환하며 앞뒤 공백은 무시.
// 첫 번째 문자열을 숫자로 해석할 수 없는 경우 NaN을 반환
parseInt('34 56 78'); // 34
parseInt('40 year'); // 40
parseInt('hello 40'); // NaN
parseInt('  40   50   ') // 40
parseInt('  40   ') // 40


// 10진수 숫자를 해당 기수의 문자열로 변환하여 반환하고 싶을 때는 Number.prototype.toString 메서드를 사용
const x = 15;

// 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환
x.toString(2) // 1111
// 문자열 '1111'을 2진수로 해석하고, 그 결과를 10진수 정수로 반환
parseInt(x.toString(2), 2) // 15
```



<br>

**encodeURI / decodeURI**

encodeURI 함수는 완전한 URI(uniform resource identifier)를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.

> URI - 인터넷에 있는 자원을 나타내는 유일한 주소

<img src="./image/built in object2.png" height="300">

> 인코딩 - URI 문자들을 이스케이프 처리하는 것

> 이스케이프 처리 - 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것

decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

```js
// 완전한 URI
const uri = 'http://example.com?name=이응모&job=programmer&teacher'

const enc = encodeURI(uri);
enc // 'http://example.com?name=%EC%9D%B4%EC%9D%91%EB%AA%A8&job=programmer&teacher'

const dec = decodeURI(enc);
dec // 'http://example.com?name=이응모&job=programmer&teacher'
```



<br>

**encodeURIComponent / decodeURIComponent**

encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩한다.

알파벳, 0~9의 숫자, -, _, ·, !, ~, *,() 문자는 이스케이프 처리에서 제외된다.

encodeURIComponent  함수는 인수로 전달된 문자열을 URI 구성요소인 쿼리 스트링의 일부로 간주한다.

따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다(encodeURI는 구분자로 사용되는 것은 인코딩하지 않음)

decodeURIComponent 함수는 매개변수로 전달된 URI 구성 요소를 디코딩한다.

```js
// URI의 쿼리 스트링
const uriComp = 'name=이응모&job=programmer&teacher'

let enc = encodeURIComponent (uriComp);
enc // 'name%3D%EC%9D%B4%EC%9D%91%EB%AA%A8%26job%3Dprogrammer%26teacher'

let dec = decodeURIComponent (enc);
dec // 'name=이응모&job=programmer&teacher'
```



<br>

<br>

### 암묵적 전역

선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문에 선언하지 않은 식별자는 선언된 전역 변수처럼 동작하는데

이러한 현상을 **암묵적 전역**이라 한다. 

하지만 이 경우는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었기 때문에 변수가 아니므로 변수 호이스팅이 발생하지 않는다.

```js
// 변수 x는 호이스팅이 발생한다
console.log(x);
// y는 프로퍼티이므로 호이스팅이 발생하지 않는다.
console.log(y); // ReferenceError: y is not defined

var x = 10;

function foo(){
    y=20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x+y) // 30

delete x ; // 전역변수는 삭제되지 않는다
delete y ; // 프로퍼티는 삭제된다.
```

