# 생성자 함수에 의한 객체 생성



## 1. Object 생성자 함수

> 생성자 함수(constructor) - new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
>
> 생성자 함수에 의해 생성된 객체를 인스턴스(instance)라 한다.
>
> 자바스크립트는 String, Number, Boolean, Fucntion, Array, Date, RegExp, Promise 등의 생성자 함수 제공

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환

빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.



```js
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function(){
    console.log(' Hi! my name is '+this.name);
}
```



```js
const num = 123;
num // 123
typeof num // 'number'

const num2 = new Number(123);
num2 // Number {123}
typeof num2 //'object'
```



<br>



## 2. 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

객체는 프로퍼티를 통해 객체 고유의 상태를 표현하고 

메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다

```js
// circle1 객체와 circle2 객체와 같이 객체 리터럴을 생성하는 경우 프로퍼티 구조가 동일해도
// 같은 프로퍼티와 메서드를 기술해야한다. -> 수십 개의 객체를 생성할 때 문제가 크다.

const circle1 = {
    radius : 5,
    getDiameter(){
        return 2* this.radius
    }
}

const circle2 = {
    radius : 10,
    getDiameter(){
        return 2* this.radius
    }
}
```



### 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼

생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능

```js
//생성자 함수
function Circle(radius){
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
    this.radius = radius; //-> 인스턴스의 radius의 값 할당이라고 보자
    this.getDiameter = function(){
        return 2 * this.radius;
    };
}

//인스턴스 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

> this - 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수(self-referencing variable)
>
> <table>
>     <th>함수 호출 방식</th>
>     <th>this가 가리키는 값(this 바인딩)</th>
>     <tr><td>일반 함수로서 호출</td><td>전역 객체</td></tr>
>     <tr><td>메서드로서 호출</td><td>메서드를 호출한 객체(마침표 앞의 객체)</td></tr>
>     <tr><td>생성자 함수로서 호출</td><td>생성자 함수가 생성할 인스턴스</td></tr>
> </table>
>
> ```js
> function foo(){
>     console.log(this);
> }
> 
> foo() // window
> 
> const obj = { foo }; // ES6 프로퍼티 축약표현
> obj.foo(); // obj
> 
> const inst = new foo(); // inst
> ```
>
> 

### 생성자 함수의 인스턴스 생성과정

생성자 함수의 역할은 프로퍼티 구조가 동일한 

인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 **인스턴스를 생성**하는 것과

**생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)**하는 것

---

생성자 함수의 내부를 보면 반환하는 코드가 보이질 않는데 자바스크립트 엔진은 암묵적으로 

인스턴스를 생성하고 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환한다.

---

```js

function Circle(radius){
   	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩 된다.
    console.log(this);	// Circle {} -> 암묵적으로 빈 객체가 생성되고 이 객체가 생성자 함수가 생성한 인스턴스다.	
    				// 암묵적으로 생성된 빈 객체, 인스턴스는 this에 바인딩(식별자와 값을 연결하는 과정)된다.
    				// 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유
    
    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    // -> this에 바인딩 되어 있는 인스턴스에 프로퍼티나 메서드 추가하고 생성자 함수가 인수로 전달받은 초기값을
    // 인스턴스 프로퍼티에 할당하여초기화하거나 고정값을 할당.
    this.radius = radius; 
    this.getDiameter = function(){
        return 2 * this.radius;
    };
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    
    // 명시적으로 객체를 반환하면 암묵적인 this가 무시된다.
    //return {} -> 인스턴스 생성시 {} 반환
    
    // 명시적으로 원시 값을 반환은 무시되고 암묵적으로 this가 반환
    // return 100;
}


const circle1 = new Circle(5); // Circle{ getDiameter : f(), radius: 5}
```



### 내부 메서드 [[call]]과 [[construct]]

함수 선언문, 함수 표현식으로 정의한 함수는 생성자 함수로서 호출할 수 있다(new 연산자와 함께 호출하여 객체를 생성하는 것)

-> 함수는 객체이므로 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모드 가지고 있다.



```js
function foo(){}

// 함수는 객체이므로 프로퍼티, 메서드 소유 가능
foo.prop = 10;
foo.method = function(){
    console.log(this.prop);
}

foo.method(); // 10
```

**일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.**

-> 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부메서드와 함수로서 동작하기 위한 

함수 객체만을 위한 [[Environment]], [[Formal Parameters]] 등의 내부 슬롯과

[[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.



---

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고,

new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.

> callable - 호출할 수 있는 객체, 즉 함수. 내부 메서드 [[Call]]을 갖는 함수 객체
>
> constructor - 생성자 함수로서 호출할 수 있는 함수. 내부 메서드 [[Construct]]를 갖는 함수 객체
>
> non-constructor  - 객체를 생성자 함수로서 호출할 수 없는 함수.[[Construct]]를 갖지 않는 함수 객체

<img src="./image/constructor function.png">

호출 할 수 없는 객체는 함수객체가 아니므로 함수 객체는 반드시 callable이어야 한다

즉 모든 함수 객체는 내부 메서드 [[Call]]을 가지고 있으므로 호출할 수 있다.

또한 함수 객체는 constructor 일 수도 있고 non-constructor 일 수 도 있으므로

모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.



### constructor 와 non-constructor의 구분

자바스크립트 엔진은 함수 정의 방식에 따라 함수 객체를 생성할 때 함수를  constructor와 non-constructor로 구분한다.

>constructor - 함수 선언문, 함수 표현식, 클래스
>
>non-constructor - 메서드(ES6 메서드 축약 표현), 화살표 함수

```js
// 일반 함수 정의 : 함수 선언문, 함수 표현식
function foo(){}
const bar = function(){};

// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수. 이는 메서드로 인정하지 않는다.
const baz = {
    x : function(){}
};

// 일반 함수로 정의된 함수만이 constructor다.
new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

// 화살표 함수 정의
const arrow = () =>{};

new arrow(); // TypeError: arrow is not a constructor

//메서드 정의 : ECMAScript 사양에서 ES6의 메서드 축약 표현만 메서드로 인정
const obj = {
    x(){}
};
new obj.x(); //TypeError: obj.x is not a constructor
```

함수가 어디에 할당되어 있는지에 따라 메서드인지를 판단하는 것이 아니라 함수 정의 방식에 따라 constructor와 non-constructor를 구분

```js
// 일반 함수(callable 이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다.
function foo(){}

// 일반함수로서 호출, [[Call]]이 호출
foo();

// 생성자 함수로서 호출, [[Construct]]가 호출
new foo();
```

### 

```js
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    };
}

const circle = Circle(5);
circle // undefined -> 반환값이 없기 때문

radius // 5 -> 일반 함수 내부의 this는 전역 객체 window를 가리키기 때문에 전역 객체의 프로퍼티와 메서드가 된다.
getDiameter() //10

circle.getDiameter() // TypeError: Cannot read properties of undefined (reading 'getDiameter')
```



### new.target

함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.

new 연산자 없이 일반 함수로서 호출된 내부의 new.target은 undefined다.

```js
function Circle(radius){
    if(!new.target){
        // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
        return new Circle(radius);
    }
    
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    };
}

// new 연산자 없이 생성자 함수 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
```





