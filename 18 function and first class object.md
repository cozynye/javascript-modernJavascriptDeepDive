# 함수와 일급 객체

## 일급 객체

> **일급 객체** - 다음과 같은 조건을 만족하는 객체
>
> 1. 무명의 리터럴로 생성(런터임에 생성 가능)
> 2. 변수나 자료구조(객체, 배열 등)에 저장 가능
> 3. 함수의 매개변수에 전달 가능
> 4. 함수의 반환값으로 사용 가능

```js
// 1. 함수는 무명의 리터럴로 생성
// 2. 함수는 변수에 저장 가능
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당
const increase = function(num){
  return ++num;
}
const decrease = function(num){
  return --num;
}

// 3. 함수는 객체에 저장 가능
const predicates = {increase, decrease};
predicates // {increase: ƒ, decrease: ƒ}

// 4. 함수의 매개변수에 전달 가능
// 5. 함수의 반환값으로 사용 가능
function makeCounter(predicates){
  let num = 0;
  return function(){
    num = predicates(num);
    return num;
  };
}

// 6. 함수는 매개변수에게 함수를 전달 가능
const increaser = makeCounter(predicates.increase);
increaser() // 1

```



<br>

## 함수 객체의 프로퍼티

```js
function square(number){
  return number * number;
}

// square 함수의 모든 프로퍼티 어트리뷰트를 확인
Object.getOwnPropertyDescriptors(square)
/*{length: {value: 1, writable: false, enumerable: false, configurable: true}, 
name: {value: 'square', writable: false, enumerable: false, configurable: true}, 
arguments: {value: null, writable: false, enumerable: false, configurable: false}, 
caller: {value: null, writable: false, enumerable: false, configurable: false}, 
prototype: {value: {…}, writable: true, enumerable: false, configurable: false}}*/

//__proto__는 squar 함수의 프로퍼티가 아니다.
Object.getOwnPropertyDescriptor(square, '__proto__') // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__') 
//{enumerable: false, configurable: true, get: ƒ, set: ƒ}
```

arguments, caller, length, name, prototype 프로퍼티는 일반 객체에는 없는 함수 객체의  고유데이터 프로퍼티

하지만 \_\_proto\_\_는 접근자 프로퍼티이며, 함수 객체의 고유 프로퍼티가 아니라 Object.prototype 객체의 프로퍼티를 상속 받은 것

Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용 가능

-> Object.prototype 객체의 \_\_proto\_\_ 접근자 프로퍼티는 모든 객체가 사용 가능



### arguments 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체

arguments 객체는 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다

-> 함수 외부에서는 참조 불가

```js
// arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타낸다
// arguments 객체의 callee 프로퍼티는 호출되어 arguments 객체를 생성한 함수, 즉 함수 자신을 가리키고 arguments 객체의 length 프로퍼티는 인수의 개수를 가리킨다

function multiply(x, y){
  console.log(arguments);
}
multiply() // Arguments [callee: ƒ, Symbol(Symbol.iterator): ƒ]
multiply(1) //Arguments [1, callee: ƒ, Symbol(Symbol.iterator): ƒ]
multiply(1,2) // Arguments(2) [1, 2, callee: ƒ, Symbol(Symbol.iterator): ƒ]

// 초과된 인수는 버려지는 것은 아니고 암묵적으로 arguments 객체의 프로퍼티로 된다.
multiply(1,2,3,4) // Arguments(4) [1, 2, 3, 4, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성 상 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 때 유용하게 사용하는 것이 arguments 객체다.

```js
// arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용
function sum(){
  let res = 0;
  
  for(let i = 0; i < arguments.length; i++){
    res +=arguments[i]
  }
  return res;
}

sum() //0
sum(1) // 1
sum(1,2) // 3
sum(1,2,3) // 6
```

arguments 객체는 배열 형태로 인자를 담고 있지만 배열이 아닌 유사배열 객체이다.

즉 배열 메서드를 사용할 경우 에러가 사용하며 Function.prototype.call, Function.prototype.apply를 사용해 간접 호출해야 한다

> 유사 배열 객체 - length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체



### caller 프로퍼티

caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티

함수 객체의 caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다

```js
function foo(func){
  return func();
}

function bar(){
  return 'caller:' + bar.caller;
}
foo(bar) // 'caller:function foo(func){\n  return func();\n}'
bar() // 'caller:null'
```



### name 프로퍼티

함수 객체의 name 프로퍼티는 함수 이름을 나타낸다

```js
var namedFunc = function foo(){};
namedFunc.name // foo

var annoymousFunc = function(){};
//ES5 : name 프로퍼티는 빈 문자열을 값으로 갖는다.
//ES6 : name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다
annoymousFunc.name // annoymousFunc

function bar(){}
bar.name // bar
```



### \_\_proto\_\_ 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다

[[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토 타입 객체를 가리킨다.

\_\_proto\_\_ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

내부 슬롯에는 직접 접근 X, 간접적인 접근 방법을 제공하는 경우에 한아여 접근 가능

[[Prototype]] 내부 슬롯에도 직접 접근 X, \_\_proto\_\_ 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있음

```js
const obj = {a : 1};

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype
obj.__proto__ === Object.prototype // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입의 객체인 Object.prototype의 프로퍼티를 상속받는다
// hasOwnProperty 메서드는 Object.prototype의 메서드
obj.hasOwnProperty('a') // true
obj.hasOwnProperty('__proto__') // false
```



### prototype 프로퍼티

prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체

즉 consturctor만이 소유하는 프로퍼티

일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다

```js
// 함수 객체는 prototype 프로퍼티를 소유
(function(){}).hasOwnProperty('prototype') // true

// 일반 객체는 prototype 프로퍼티 소유 X
({}).hasOwnProperty('prototype') // false
```

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.