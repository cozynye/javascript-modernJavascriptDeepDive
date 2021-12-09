

# 프로토타입

자바스크립트는 객체 기반의 프로그래밍 언어이며 **자바스크립트를 이루고 있는 거의 \"모든 것\"이 객체**다.



## 1. 객체지향 프로그래밍

> 객체지향 프로그래밍 - 여러 개의 독립적인 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임

```js
// 이름과 주소 속성을 갖는 객체
const person = {
    name : 'Lee',
    address : 'seoul'
}
```

프로그래머(subject, 주체)는 이름과 주소 속성으로 표현된 객체(obejct)인 person을 다른 객체와 구별하여 인식할 수 있다.

> 객체 - 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조

객체지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그램이 패러다임이다.

```JS
// 반지름은 원의 상태를 나타내는 데이터이며, 원의 지름, 둘레, 넓이를 구하는 것은 동작이다.
const circle = {
    radius : 5,
    getDiameter(){
        return 2 * this.radius;
    }
    getPerimeter(){
        return 2 * Math.PI * this.radius;
    }
	getArea(){
        return Math.PI * this.radius ** 2;
    }
}
```



위의 circle 객체로 본다면 객체지향 프로그래밍은 객체의 **상태**를 나타내는 데이터와 

상태 데이터를 조작할 수 있는 **동작**을 하나의 논리적인 단위로 묶어 생각한다.

즉 객체는 **상태 데이터**(property)와 **동작**(method)을 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.

또한 객체는 고유의 기능을 갖는 독립적인 것으로 볼 수 있지만 자신의 고유한 기능을 수행하면서 다른 객체와 관계성을 가질 수 있다.



<br><br>

## 2. 상속과 프로토타입

> 상속 - 객체지향 프로그래밍의 핵심 개념
>
> 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

```js
// 생성자 함수
function Circle(radius){
    this.radius = radius;
    this.getArea = function(){
        return Math.PI * this.radius ** 2;
    };
}

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때 마다 동일한 동작을 하는 
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// -> getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
circle1.getArea === circle2.getArea // false
```

```js
// 생성자 함수
function Circle(radius){
    this.radius = radius;
    
    // Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를 공유해서 사용할 수 있도록 프로토 타입에 추가
    // 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
    Circle.prototype.getArea = function(){
        return Math.PI * this.radius ** 2;
    };
}

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// -> Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유
circle1.getArea === circle2.getArea // true
```

Circle 생성자 함수가 생성한 모든 인스턴스는 상위 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.

getArea 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype의 메서드로 할당

-> 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고 내용이 동일한 메서드는 상속을 통해 공유하여 사용하는 것

> 상속은 코드의 재사용이란 관점에서 매우 유용
>
> 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 **프로퍼티**나 **메서드**를 프로토타입에 미리 구현해 두면 
>
> 생성자 함수가 생성할 모든 인스턴스는 별도의 구현 없이 상위 객체인 프로터타입의 자산 사용 가능

<br><br>

## 3. 프로토타입 객체

프로토타입 객체는 상속을 구현하기 위해 사용된다.

프로토타입은 어떤 객체의 상위 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티, 메서드를 제공한다.

프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 사용 가능



---

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 내부 슬롯의 값은 프로토타입의 참조이다.

[[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정되는데 

즉, 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.

-> 객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype이고

생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

---

<br>

### \_\_proto\_\_ 접근자 프로퍼티

모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로퍼티, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근 가능

```js
const person = {
    name : 'LEE',
    age: 18
};
console.log(person);
// {name: 'LEE', age: 18}
// age: 18
// name: "LEE"
// 아래부터 person 객체의 프로토타입인 Object.prototype
// __proto__ 접근자 프로퍼티를 통해 person 객체의 [[Prototype]] 내부 슬롯이 가리키는 객체인
// Object.prototype에 접근한 결과를 콘솔에 표시한 것
// [[Prototype]]: Object
// constructor: ƒ Object()
// hasOwnProperty: ƒ hasOwnProperty()
// isPrototypeOf: ƒ isPrototypeOf()
// propertyIsEnumerable: ƒ propertyIsEnumerable()
// toLocaleString: ƒ toLocaleString()
// toString: ƒ toString()
// valueOf: ƒ valueOf()
// __defineGetter__: ƒ __defineGetter__()
// __defineSetter__: ƒ __defineSetter__()
// __lookupGetter__: ƒ __lookupGetter__()
// __lookupSetter__: ƒ __lookupSetter__()
// __proto__: (...)
// get __proto__: ƒ __proto__()
// set __proto__: ƒ __proto__()
```

---

<br>

**\_\_proto\_\_는 접근자 프로퍼티이다**

접근자 프로퍼티는 자체적으로 값([[Value]] 프로퍼티 어트리뷰트)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때

사용하는 접근자 함수, 즉 [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티

```js
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__')
// configurable: true
// enumerable: false
// get: ƒ __proto__()
// set: ƒ __proto__()
// [[Prototype]]: Object

// Object.prototype의 접근자 프로퍼티인 __proto__는 getter/setter 함수라고 부르는 접근자 함수
//([[Get]], [[Set]] 프로퍼티 어트리뷰트에 할당된 함수) 를 통해 [[Prototype]] 내부 슬롯의 값,
// 프로토타입을 취득하거나 할당한다.
```

```js
const obj = {};
const parent = {x:1};

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;

// setter 함수인 set__proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent

obj.x // 1
```

---

<br>

**\_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용된다**.

\_\_proto__ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.

모든 객체는 상속을 통해 Object.prototype.\__proto__ 접근자 프로퍼티를 사용할 수 있다.

```js
const person = { name:'Lee'};

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
person.hasOwnProperty('__proto__') // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__') 
//{enumerable: false, configurable: true, get: ƒ, set: ƒ}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용 가능
person.__proto__ == Object.prototype // true
```

---

<br>

**\__proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

[[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에

의해 프로토타입 체인이 생성되는 것을 방지하기 위해서이다.

```js
const child = {}
const parent = {}

child.__proto__ = parent
parent.__proto__ = child // TypeError: Cyclic __proto__ value
// 에러 없이 처리된다면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이
// 만들어지기 때문에 __proto__ 접근자 프로퍼티는 에러를 방생시킨다.
```

프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.

서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인, 순환 참조(circular reference)하는 프로토타입 체인이

만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 프로토타입 체인에서 프로퍼티 검색할 때 무한 루프에 빠진다.

-> 아무런 체크 없이 프로토타입을 교체할 수 없도록 \__proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현

---

<br>

**\__proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장되지 않는다.**

\__proto__ 접근자 프로퍼티는 ES6에서 \_\_proto__를 표준으로 채택

모든 객체가 \_\_proto__ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문에 코드 내에서 \_\_proto__ 접근자

프로퍼티를 직접 사용하는 것은 권장하지 않는다.

```js
// obj는 프로토타입 체인의 종점 -> Object.__proto__를 상속받을 수 없다
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
obj.__proto__ // undefined
```

```js
// __proto__ 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는
// Object.getPrototypeOf 메서드를 사용하고,
// 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용

const obj={};
const parent = {x:1};

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); 
// {constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}

// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); //{}
```



### 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

> non-constructor - 객체를 생성자 함수로서 호출할 수 없는 함수.[[Construct]]를 갖지 않는 함수 객체(메서드, 화살표 함수)

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function(){}).hasOwnProperty('prototype'); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // false
```

```js
// 화살표 함수와, 메서드 축약 표현으로 정의한 메서드는 prototype을 소유하지 않으며 
// 프로토타입도 생성하지 않는다
const Person = name =>{
    this.name = name;
};
const obj = {
    foo(){}
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
Person.hasOwnProperty('prototype'); // false
obj.foo.hasOwnProperty('prototype'); // false

// non-constructor는 프로토타입을 생성하지 않는다.
Person.prototype; // undefined
obj.foo.prototype; //undefined
```

---

모든 객체가 가지고 있는(Object.prototype으로부터 상속받은) \_\_proto\_\_ 접근자 프로퍼티와 함수 객체만이 가지고 있는

prototype 프로퍼티는 동일한 프로토타입을 가리킨다. 하지만 이들 프로퍼티를 사용하는 주체가 다르다.

<table>
    <th>구분</th>
    <th>소유</th>
    <th>값</th>
    <th>사용 주체</th>
    <th>사용 목적</th>
    <tr>
    	<td>__proto__ 접근자 프로퍼티</td>
        <td>모든 객체</td>
        <td>프로토타입의 참조</td>
        <td>모든 객체</td>
        <td>객체자 자신의 프로토타입에 접근 또는 교체하기 위해 사용</td>
    </tr>
     <tr>
    	<td>prototype 프로퍼티</td>
        <td>constructor</td>
        <td>프로토타입의 참조</td>
        <td>생성자 함수</td>
        <td>생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용</td>
    </tr>
</table>

```js
// 생성자 함수
function Person(name){
    this.name = name;
}

const me = new Person('Lee');

// Person.prototype과 me.__proto__는 동일한 프로토타입을 가리킨다.
Person.prototype === me.__proto__ // true
```



<br>

### 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 가지며 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

```js
// 생성자 함수
function Person(name){
    this.name = name;
}

const me = new Person('Lee');

me.constructor === Person // true
```

me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결

me 객체에는 constructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototype에는 constructor 프로퍼티가 있다.

me 객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있다.

<img src=./image/prototype1.png height=500>



<br>

<br>

## 4. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.

이때 constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수다.

```js
// obj 객체를 생성한 생성자 함수는 Object다
const obj = new Object();
obj.constructor === Object // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function('a', 'b', 'return a + b');
add.constructor === Function // true

// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');
me.constructor === Person // true
```

> **리터럴**은 사람이 이해할 수 있는 문자(숫자, 알파벳, 한글 등) 또는 미리 약속된 기호('', "", ., [], {} 등)로 표기한 코드
> 자바스크립트 엔진은 코드가 실행되는 시점인 런타임에 리터럴을 평가해 값을 생성

```js
// 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께
// 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성방식도 있다.


// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function(a,b){return a+b};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규 표현식 리터럴
const regexp = /is/ig;
```

---

리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재한다.

하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시

객체를 생성한 생성자 함수라고 단정할 수는 없다.

```js
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성
const obj = {};

// obj 객체의 생성자 함수는 Object 생성자 함수다.
obj.constructor === Object // true

// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴에 의해 생성된 객체
// 하지만 obj 객체는 Object 생성자 함수와 constructor 프로퍼티로 연결되어 있다.
```

> ECMAScript 사양을 본다면 Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서
>
> 호출하면 내부적으로는 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로
>
> 갖는 빈 객체를 생성한다.

>함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
>
>new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.
>
>new 연산자 없이 일반 함수로서 호출된 내부의 new.target은 undefined다.

```js
// Object 생성자 함수에 의한 객체 생성
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다
let obj = new Object();
obj // {}

// new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성
class Foo extends Object {}
new Foo(); // Foo {}

// 인수가 전달된 경우에는 인수를 객체로 변환한다
// Number 객체 생성
obj = new Object(123);
obj // Number {123}

// String 객체 생성
obj = new Object('123');
obj // String {123}
```

객체 리터럴이 평가될 때는  추상 연산 OrdinaryObjectCreate를 호출하여 빈객체를 생성, 프로퍼티를 추가하도록 정의 되있다.

Object 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하는

점에서 동일하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다

즉 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.

---

```js
// foo 함수는 Fucntion 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성
function foo(){}

// constructor 프로퍼티를 통해 확인하면 foo의 생성자 함수는 Function 생성자 함수다.
foo.constructor === Function // true
```

함수 객체의 경우도 Function 생성자 함수를 호출하여 생성한 함수는 렉시컬 스코프를 만들지 않고 전역 함수인 것처럼

스코프를 생성하며 클로저도 만들지 않는다.

따라서 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 Function 생성자 함수가 아니다.

하지만 constructor 프로퍼티를 통해 확인하면 foo 함수의 생성자 함수는 Function 생성자 함수다.

---

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요

따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수는 갖는다.

프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문

-> **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다**.

---

리터럴 표기법에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니지만 큰 틀에서 생각해보면 리터럴 표기법으로

생성한 객체도 생성자 함수로 생성한 객체와 본질적인 면에서 큰 차이는 없다.

객체 리터럴에 의해 생성한 객체와 Object 생성자 함수에 의해 생성한 객체는 생성 과정에 미묘한 차이는 있지만

결국 객체로서 동일한 특성을 갖는다.

즉, 프로토 타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를

생성한 생성자 함수로 생각해도 큰 문제는 없다

<br>

<table>
    <th>리터럴 표기법</th>
    <th>생성자 함수</th>
    <th>프로토타입</th>
    <tr>
    	<td>객체 리터럴</td>
        <td>Object</td>
        <td>Object.prototype</td>
    </tr>
	 <tr>
    	<td>함수 리터럴</td>
        <td>Function</td>
        <td>Function.prototype</td>
    </tr>
     <tr>
    	<td>배열 리터럴</td>
        <td>Array</td>
        <td>Array.prototype</td>
    </tr>
     <tr>
    	<td>정규 표현식 리터럴</td>
        <td>RegExp</td>
        <td>RegExp.prototype</td>
    </tr>
    <caption>리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입</caption>
</table>



<br>

<br>

## 5. 프로토타입의 생성 시점

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**

-> 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문



<br>

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

화살표 함수나 메서드 축약 표현으로 정의하지 않고 일반함수(함수 선언문, 함수 표현식)으로 정의한 함수 객체는

new 연산자와 함께 생성자 함수로서 호출할 수 있다.



**생성자 함수로서 호출할 수 있는 함수(constructor)는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성**

```js
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성
// 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행
// Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 되고 이때 프로토타입도 생성된다.
// 생성된 프로토타입은 Person 생성자 함수의 prototype 프로퍼티에 바인딩된다
Person.prototype // {constructor: ƒ}
				// constructor: ƒ Person(name)
				//[[Prototype]]: Object

function Person(name){
    this.name = name;
}

// non-constructor는 프로토타입이 생성되지 않는다.
const Person2 = name => {
    this.name = name;
};

Person2.prototype // undefined
```

위의 예처럼 빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에

프로토타입도 생성되며, 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.



<br>

### 빌트인 생성자 함수와 프로토타입 생성 시점

Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 일반 함수와 같이

빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성되고 생성된 프로토타입은 빌트인 생성자 함수의

prototype 프로퍼티에 바인딩 된다.



<br>

<br>

## 6. 객체 생성 방식과 프로토타입의 결정

> 객체 생성 방식
>
> - 객체 리터럴
> - Object 생성자 함수
> - 생성자 함수
> - Object.create 메서드
> - 클래스(ES6)

위처럼 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

추상 연산 OrdinaryObjectCreate는 자신이 생성할 객체의 프로토타입을 인수로 전달받고 자신이 생성할 객체에

추가할 프로퍼티 목록을 옵션으로 전달 할 수 있다.

추상 연산 OrdinaryObjectCreate는 빈객체를 생성한 후, 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가,

인수로 전달받은 프로토타입을 자신이 생성한 객체의 [[Prototype]] 내부 슬롯에 할당한 다음, 생성한 객체를 반환한다.

-> 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정되고

이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.



<br>

### 객체 리터럴에 의해 생성된 객체의 프로토타입

자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출한다.

이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```js
const obj = {x:1};

obj.constructor === Object // true
obj.hasOwnProperty('x') // true
```

위 객체 리터럴이 평가되면 추상 연산 OrdinaryObjectCreate에 의해 아래의 그림과 같이 Object 생성자 함수와

Object.prototype과 생성된 객체 사이에 연결이 만들어 진다.

<img src=./image/prototype2.png height=500>

 위처럼 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게되며, OBject.prototype을 상속받는다.

obj 객체가 프로토타입인 Object.prototype 객체를 상속 받았기 때문에 Object.prototype 의 프로퍼티, 메서드를 자유롭게 사용 가능하다.



<br>

### Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수를 인수없이 호출하면 빈 객체가 생성되고 

Object 생성자 함수를 호출하면 추상 연산 OrdinaryObjectCreate가 호출된다.

이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이며

Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.



```js
const obj = new Object();
obj.x = 1;

// 코드가 실행되면 추상 연산 에 의해 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어진다.
```

<img src=./image/prototype3.png height=500>

이처럼 Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, Object.prototype을 상속받는다.

```js
const obj = new Object();
obj.x = 1;

obj.constructor === Object // true
obj.hasOwnProperty('x') // true
```

객체 리터럴 방식을 객체 리터럴 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후

프로퍼티를 추가한다는 객체를 생성하고 프로퍼티를 추가하는 방식의 차이가 있다.



<br>

### 생성자 함수에 의해 생성된 객체의 프로토타입

new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 추상 연산 OrdinaryObjectCreate가 호출된다.

이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어있는 객체다.

```js
function Person(name){
    this.name = name;
}

const me = new Person('Lee');
```

위 코드가 실행되면 추상 연산 OrdinaryObjectCreate에 의해 아래와 같이 생성자 함수와 생성자 함수의 prototype 프로퍼티에 바인딩

되어 있는 객체와 생성된 객체 사이에 연결이 만들어진다.

<img src=./image/prototype4.png height=300>



빌트인 객체인 Object 생성자 함수와 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메서드를 갖고 있다.

하지만 사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor뿐이다.

그러므로 필요시 프로토타입 Person.prototype에 프로퍼티를 추가/삭제하여 하위 객체가 상속받을 수 있도록 구현할 수 있다.

```js
function Person(name){
    this.name = name;
}

// 프로토타입 메서드 추가
Person.prototype.sayHello = function(){
    console.log(`Hi! my name is ${this.name}`)
}

const person = new Person('Lee');

person.sayHello(); // Hi! my name is Lee
```



<br>

<br>

## 7. 프로토타입 체인

자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부

슬롯의 참조를 따라 상위의 프로토타입의 프로퍼티를 순차적으로 검색하는데 이를 **프로토타입 체인**이라 한다.

프로토타입 체인의 최상위에 위치하는 객체는 언제나 **Object.prototype**이다.

따라서 모든 객체는 Object.prototype을 상속받으며 **Object.prototype을 프로토타입 체인의 종점**이라 한다.



<br>

<br>

## 8. 오버라이딩과 프로퍼티 섀도잉

```js
const Person = (function(){
    
    //생성자 함수
    function Person(name){
    this.name = name;
	}	
    
    // 프로토타입 메서드 추가
	Person.prototype.sayHello = function(){
    console.log(`Hi! my name is ${this.name}`);
	};
    
    // 생성자 함수를 반환
    return Person;
}());

const me = new Person('Lee');

me.sayHello() // Hi! my name is Lee

// 인스턴스 메서드 추가
me.sayHello = function(){
    console.log(`Hey! my name is ${this.name}`);
}

me.sayHello() // Hey! my name is Lee
```

<img src=./image/prototype5.png height=500>

프로토타입이 소유한 프로퍼티(메서드 포함)을 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라 한다.

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여

프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다.

이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다

이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 **프로퍼티 섀도잉**이라 한다.

> 오버라이딩 - 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

> 오버로딩 - 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해
>
> 메서드를 구별하여 호출하는 방식

```js
// 메서드를 삭제하는 경우

me.sayHello(); // Hey! my name is Lee

delete me.sayHello

me.sayHello(); // Hi! my name is Lee

delete me.sayHello
// 메서드가 삭제되지 않고 프로토타입의 메서드가 호출된다.
me.sayHello(); // Hi! my name is Lee
```

위와 같이 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다.

즉 하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set액세스는 허용되지 않는다.

-> 프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 

직접 접근해야 한다

```js
// 프로토타입 변경
Person.prototype.sayHello = function(){
    console.log(`Hey! my name is ${this.name}`);
};

me.sayHello(); // Hey! my name is Lee

delete PErson.prototype.sayHello;
me.sayHello(); // TypeError:  me.sayHello is not a function
```





<br>

<br>

## 9. 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경할 수 있다.

즉 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미하는데 이러한 특징을 활용하여 객체 간의

상속 관계를 동적으로 변경 가능하다.



<br>

### 생성자 함수에 의한 프로토타입 교체

```js
const Person = (function(){
    function Person(name){
        this.name = name;
    }
    
      return Person;
}());

const me = new Person('Lee')

me //Person {name: 'Lee'}
	//name: "Lee"
	//[[Prototype]]: Object
	//	constructor: ƒ Person(name)
	//	[[Prototype]]: Object
```

위, 아래 예제 차이를 살펴보자

```js
const Person = (function(){
    function Person(name){
        this.name = name;
    }
    
    // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
    // Person.prototype에 객체 리터럴을 할당 -> Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체
    Person.prototype = {
        sayHello(){
             console.log(`Hi! my name is ${this.name}`);
        }
    };
    return Person;
}());

const me = new Person('Lee');
me // Person {name: 'Lee'}
		//name: "Lee"
		//[[Prototype]]: Object
		//		sayHello: ƒ sayHello()
		//		[[Prototype]]: Object
```



<img src=./image/prototype6.png height=400>

프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다.

constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티다.

따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

```js
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
me.constructor === Person // false

// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
me.constructor === Object // true
```

파괴된 constructor 프로퍼티와 생성된 함수 간의 연결을 되살리기 위해서는 constructor 프로퍼티를 추가하는 것이다

```js
const Person = (function(){
    function Person(name){
        this.name = name;
    }
    
    // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
    Person.prototype = {
        // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
        constructor : Person,
        sayHello(){
             console.log(`Hi! my name is ${this.name}`);
        }
    };
    return Person;
}());

const me = new Person('Lee');

me.constructor === Person // true
me.constructor === Object // false
```



<br>

### 인스턴스에 의한 프로토타입의 교체

프로토타입은 생성자 함수의 prototype 프로퍼티뿐만 아니라 인스턴스의 \_\_proto__ 접근자 프로퍼티

(또는 Object.setPrototypeOf 메서드)를 통해 프로토타입을 교체할 수 있다.

\_\_proto__ 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것!

```js
function Person(name){
    this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
    sayHello(){
        console.log(`Hi! my name is ${this.name}`);
    }
}
me.sayHello() // TypeError: me.sayHello is not a function

// me 객체의 프로토타입을 parent 객체로 교체
Object.setPrototypeOf(me, parent);
// me.__proto__ = parent; -> 바로 위의 코드와 동일하게 동작

me.sayHello() // Hi! my name is Lee
```

생성자 함수에 의한 프로토타입의 교체와 마찬가지로 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로

constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

그래서 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

```js
// 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재설정하여
// 파괴된 생성자 함수와 프로토타입 간의 연결을 정상으로 바꿀 수 있다.
 Person.prototype = {
        // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
        constructor : Person,
        sayHello(){
             console.log(`Hi! my name is ${this.name}`);
        }
    };
    return Person;
}());

Person.prototype = parent;
```



<br>

<br>

## 10. instanceof 연산자

instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자로 받는다

우변의 피연산자가 함수가 아닌 경우 TypeError가 발생

> **객체 instanceof 생성자 함수**

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가,

그렇지 않으면 false로 평가된다

```js
function Person(name){
    this.name = name;
}

const me = new Person('Lee');

me instanceof Person // true
me instanceof Object // true
```



instaceof 연산자는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.



<br>

<br>

## 12. 직접 상속

### Object.create에 의한 직접 상속

Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성

Object.create 메서드도 추상 연산 OrdinaryObjectCreate를 호출

---

Object.create 메서드의 첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달.

두 번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달.(생략 가능)

```js
// 프로토타입이 null인 객체를 생성하고 이 객체는 프로토타입 체인의 종점에 위치한다.
let obj = Object.create(null);
Object.getPrototypeOf(obj) === null //true

// Object.prototype을 상속받지 못한다
obj.toString(); //TypeError: obj.toString is not a function

// obj -> Object.prototype -> null
// obj = {x:1} 와 동일하다
obj = Object.create(Object.prototype,{
    x : {value:1, writable : true, enumerable : true, configurable : true}
});
// 위와 동일한 코드
// obj = Object.create(Object.prototype);
// obj.x=1;

obj.x // 1
Object.getPrototypeOf(obj) === Object.prototype // true

const myProto = {x:10};
// 임의의 객체를 직접 상속
// obj -> myProto -> Object.prototype -> null
obj = Object.create(myProto);
obj.x // 10
Object.getPrototypeOf(obj) === myProto // true

function Person(name){
    this.name = name;
}
// obj -> Person.prototype -> Object.prototype -> null
// obj = new Person('Lee')와 동일
obj = Object.create(Person.prototype);
obj.name = 'Lee';
Object.getPrototypeOf(obj) === Person.prototype // true
```

위처럼 Object.create 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성

-> 객체를 생성하면서 직접적으로 상속을 구현하는 것이다

이렇게 하면 new 연산자 없이 객체를 생성하면서 프로토타입을 지정하면서 객체를 생성하고 객체 리터럴에 의해

생성된 객체도 상속을 받을 수 있는 장점이 있다.



<br>

### 객체 리터럴 내부에서 \_\_proto__에 의한 직접 상속

객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.

```js
const proto = {x:1};

const obj = {
    y: 20,
    
    // obj --> proto -> Object.prototype -> null
    __proto__ : proto
};
// 위와 동일한 코드
/* const obj = Object.create(proto,{
 y : {value : 20, writable: true, enumerable : true, configurable: true}
});
*/

Object.getPrototypeOf(obj) === proto // true
```



<br>

<br>

## 12. 정적 프로퍼티/메서드

정정 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드이다.

```js
function Person(name){
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function(){
    console.log(`Hi, my name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = 'static prop';

// 정적 메서드
Person.staticMethod = function(){
    console.log('static method');
};

const me = new Person('Lee');

//생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod() // static method

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상 존재해야함
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드 소유 가능

Person 생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드라한다.

정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.

<img src=./image/prototype7.png height=500>

생성자 함수가 생성한 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메서드에 접근할 수 있지만

정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 접근 불가능하다.



<br>

<br>

## 13 프로퍼티 존재 확인

### in 연산자

in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인

> ```js
> //key : 프로퍼티 키를 나타내는 문자열
> //object : 객체로 평가되는 표현식
> key in object
> ```

```js
const person = {
    name : 'Lee',
    age : 18
};

'name' in person // true
'age' in person // true
'address' in person // false

// 객체가 상속받은 모든 프로토타입의 프로퍼티 확인하므로 주의 필요
'toString' in person // true
```

```js
// ES6 에서 도입된 Reflect.has 메서드는 in 연산자와 동일하게 동작
const person = {
    name : 'Lee',
    age : 18
};

Reflect.has(person, 'name') // true
Reflect.has(person, 'toString') // true
```



<br>

### Object.prototypehasOwnProperty 메서드

Object.prototype.hasOwnProperty 메서드를 사용해도 객체에 특정 프로퍼티 존재하는지 확인 가능

```js
const person = {
    name : 'Lee',
    age : 18
};

person.hasOwnProperty('name') // true

// 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우만 true 반환 상송받은 프로퍼티 키는 false 반환
person.hasOwnProperty('toString') // false
```



<br>

<br>

## 14. 프로퍼티 열거

### for ... in 문

객체의 모든 프로퍼티를 순회하며 열거하려면 for...in 문을 사용한다.

> for ( 변수선언문 in 객체){...}

```js
const person = {
    name : 'Lee',
    age : 18
};

for(let x in person){console.log(x + ' : ' + person[x])}
// name : Lee
// age : 18
```

for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의

값이 true인 프로퍼티를 순회하며 열거한다.

```js
const sym = Symbol();
const person = {
	name: 'Lee',
    address:'Seoul',
    __proto__:{age:20},
    [sym] : 10
};

for(let x in person){console.log(x + ' : ' + person[x])}
// name : Lee
// address : Seoul
// age : 20
// 심벌인 키는 열거하지 않는다.
```

상속받은 프로퍼티를 제외하고 객체 자신의 프로퍼티만 열거하려면 Object.prototype.hasOwnProperty 메서드를 사용하여

자신의 프로퍼티인지 확인해야 한다.

```js
const person = {
	name: 'Lee',
    address:'Seoul',
    __proto__:{age:20}
};

for(const key in person){
    if(!person.hasOwnProperty(key)) continue;
   console.log(key + ' : ' + person[key]);
}
```

for ... in 문은 프로퍼티를 열거할 때 순서를 보장하지 않으므로 주의 해야 한다.

하지만 대부분의 모던 브라우저는 순서를 보장하고 숫자(문자열)인 프로퍼티 키에 대해서는 정렬을 실시한다.

```js
const obj ={
    2:2,
    3:3,
    1:1,
    b:'b',
    a:'a'
}

for(const key in obj){
    if(!obj.hasOwnProperty(key)) continue;
   console.log(key + ' : ' + obj[key]);
}
// 1 : 1
// 2 : 2
// 3 : 3
// b : b
// a : a
```

배열에는 for ...in 문을 사용하지 말고 일반적인 for문이나 for ...of 문 또는 Array.prototype.forEach 메서드 사용을 권장하고

배열도 객체이므로 프로퍼티와 상속받은 프로퍼티가 포함될 수 있다.

```js
const arr = [1,2,3];
arr.x=10;

arr // [1, 2, 3, x: 10]
for(const i in arr){
    console.log(i, arr[i])
}
// 0 1
// 1 2
// 2 3
// x 10

for(let i = 0; i<arr.length;i++){
    console.log(arr[i])
}
// 1
// 2
// 3

arr.forEach(v=> console.log(v))
// 1
// 2
// 3

for(const v of arr){
    console.log(v);
}
// 1
// 2
// 3
```



<br>

### Object.key/values/entries메서드

객체 자신의 고유 프로퍼티만 열거하기 위해서는 상속받는 프로퍼티도 열거하는 for ... in 문을 사용하는 것보다는

Object.keys/values/entries 메서드를 사용하는 것을 권장함.

->

**Object.keys**메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.

**Object.values**메서드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.

**Object.entries**메서드는 객체 자신의 열거가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환

```js
const person = {
	name: 'Lee',
    address:'Seoul',
    __proto__:{age:20}
};
Object.keys(person); // ['name', 'address']
Object.values(person); // ['Lee', 'Seoul']
Object.entries(person); [['name', 'Lee'],['address', 'Seoul']]

Object.entries(person).forEach(([k,v])=> console.log(k,v))
// name Lee
// address Seoul
```

