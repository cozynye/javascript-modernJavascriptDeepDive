# let, const 키워드와 블록 레벨 스코프



## 1. var 키워드로 선언한 변수의 문제점



#### 변수 중복 선언 허용

```js
var x = 1;
var y = 2;

//var 키워드로 선언된 변수는 스코프 내에서 중복 선언 허용
// 초기화문 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드 없는 것처럼 동작
var x = 100;

//초기화문이 없는 변수 선언문의 무시됨.
var y;

x // 100
y // 2
```



#### 함수 레벨 스코프

var 키워드로 선언한 변수는 함수의 코드 블록만은 지역 스코프로 인정(함수 내부 제외한 나머지는 모두 전역 변수)

```js
var i = 1;

for(var i = 0; i < 5, i++){
    console.log(i);
}

i // 5
```



#### 변수 호이스팅

> 변수 호이스팅 - 변수 선언문이 코드의 선두로 올려진 것처럼 동작하는 자바스크립트 고유의 특징

var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 올려진 것처럼 동작해 

var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.(단 할당문 이전에 참조하면 undefined 반환)

```js
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다(2. 초기화 단계)
console.log(foo); // undefined

//변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo) // 123

// 변수 선언문
var foo;
```

변수 선언문 이전에 변수를 참조하는 것은 변수 호이스팅에 의한 에러는 나지 않지만 프로그램 흐름상 맞지 않고 가독성 떨어트려 오류를 발생시킬 수 있다.

<br>



## 2. let 키워드

#### 변수 중복 선언 금지

let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

let 이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 X

```js
let i = 1;

let i = 3; // SyntaxError: Identifier 'i' has already been declared
```

​	

#### 블록 레벨 스코프

let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```js
let i = 0; // 전역 스코프

function foo(){
    let i = 3; // 함수 레벨 스코프
    
    for( let i = 0 ; i < 5; i++){ // 블록 레벨 스코프
        console.log(i) // 0 1 2 3 4
    }
    
    console.log(i) // 3
}

i // 0
```



#### 변수 호이스팅

var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행되어 변수 선언문 이전에 변수를 참조할 수 있다.

---

let 키워드로 선언한 변수는 **'선언단계'와 '초기화 단계'가 분리**되어 진행

-> 런타임 이전에 자바스크립트 엔진에 의해 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행

---

```js
// 런타임 이전에 선언단계가 실행 되지만 초기화되지 않았다.
foo // ReferenceError : foo is not defined

let foo; // 변수 선언문에 의해 초기화 단계 실행

foo // undefined

foo=1; // 할당문에서 할당 단계 실행

foo // 1
```



```js
let num = 1;
{
console.log(num) // 1
}

//let 키워드로 선언한 변수의 경우 변수 호이스팅이 발생하지 않는다면 전역 변수 foo의 값을 참조해야하지만 
//let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 에러가 난다.
let foo = 3;
{
console.log(foo); //ReferenceError: Cannot access 'foo' before initialization
let foo;
}
```



#### 전역 객체와 let

var 키워드로 선언한 전역 변수와 전역 함수, 선언하지 않는 변수에 값을 할당한 

암묵적 전역은 전역 객체 window의 프로퍼티가 된다.(전역 객체의 프로퍼티를 참조할 때 window 생략 가능)

```js
//예제는 브라우저 환경에서 실행

var x = 1;
y = 2;
function foo(){}

window.x // 1
x // 1

window.y // 2
y // 2

window.foo // ƒ foo(){}
foo // ƒ foo(){}
```

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다

```js
// 예제는 브라우저 환경에서 실행
let x = 1;
x // 1
window.x // undefined
```



<br>

## 3. const 키워드

const 키워드는 상수를 선언하기 위해 사용(반드시 상수만을 위해서는 아니다)



#### 선언과 초기화

const 키워드로 선언한 변수는 반드신 선언과 동시에 초기화 해야 한다.

```js
const num; // SyntaxError: Missing initializer in const declaration
const num = 10;
```

const 변수는 블록 레벨 스코프를 가지며,  변수 호이스팅이 발생하지 않는 것처럼 동작한다.(변수 호이스팅 일어난다.)

```js
{
	console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
    const foo = 1;
    console.log(foo); // 1
}

console.log(foo) ; // ReferenceError: foo is not defined
```

#### 재할당 금지

const 키워드로 선언한 변수는 재할당이 금지된다.

```js
const num = 1;
num = 3; // TypeError: Assignment to constant variable.
```

#### 상수

> 상수 - 재할당이 금지된 변수

const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 

const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.

> 원시 타입에는 number, string, boolean, undefined, null, symbol이 있다

```js
// 일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다.
// 여러 단어일 경우는 언더스코어로 구분해 스네이크 케이스로 표현하는 것이 일반적이다.

const TAX_RATE = 0.1;
```

#### const 키워드와 객체

const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.

변경 가능한 값인 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.

```js
const person = {
    name : 'Lee'
};
person.name='Kim';

person // {name: 'Kim'}
```

---

변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우 스코프를 좁게하여 한정해 사용하는 것이 좋다.

---