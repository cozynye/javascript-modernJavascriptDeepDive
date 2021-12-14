# this

## 1. this 키워드

> 객체 - 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조

**this**는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**(self-referencing variable)다.

this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

<br>

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조 가능하며, 

함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다. 

this가 가리키는 값, 즉 this 바인딩(식별자와 값을 연결하는 과정)은 함수 호출 방식에 의해 동적으로 결정된다.

```js
// 객체 리터럴의 메서드 내부에서는의 this는 메서드를 호출한 객체, 즉 circle을 가리킨다.
const circle = {
    radius : 5,
    getDiameter(){
        //this는 메서드를 호출한 객체를 가리킨다
        return 2 * this.radius
    }
};

circle.getDiameter(); // 10
```

```js
// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
function Circle(radius){
    this.radius = radius;
}

Circle.prototype.getDiameter = function(){
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return this.radius*2;
}

// 인스턴스 생성
const circle = new Circle(5);
circle.getDiameter();//10
```

자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.

```js
// this는 어디서든지 참조 가능
// 전역에서 this는 전역 객체 window를 가리킨다
console.log(this); // window

function square(number){
    // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
    console.log(this); // window
    return number * number;
}

square(2);

const person = {
    name : 'lee',
    getName(){
        // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
        console.log(this); // {name: 'lee', getName: ƒ}
        return this.name
    }
};
person.getName();

function Person(name){
    this.name = name;
    //생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    console.log(this); // Person {name: 'lee'}
}

new Person('lee');
```



<br>

<br>

<br>

## 2. 함수 호출 방식과 this 바인딩

this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 **동적**으로 결정된다.

> 함수를 호출 하는 방식
>
> - 일반 함수 호출
> - 메서드 호출
> - 생성자 함수 호출
> - Fucntion.prototype.apply/call/bind 메서드에 의한 간접 호출

```js
const foo = function(){
    console.dir(this);
}
// 일반 함수 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다
foo(); // window

// 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = {foo};
obj.foo(); // Object

// 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name : 'bar'};

foo.call(bar); // Object
foo.apply(bar); // Object
foo.bind(bar)(); // Object
```

<br>

<br>

### 일반 함수 호출

기본적으로 일반 함수로 호출된 모든 함수(중첩함수, 콜백 함수 포함) 내부의 this에는 **전역 객체**가 바인딩된다.

```js
// var 키워드로 선언한 전역변수 value는 전역 객체의 프로퍼티다
var value = 1;

const obj = {
	value : 100,
    foo(){
        console.log("foo's this : ", this); // {value: 100, foo: ƒ}
        console.log("foo's this.value : ", this.value); // 100
        
        // 메서드 내에서 정의한 중첩 함수
        function bar(){
            console.log("bar's this : ", this); // window
            console.log("bar's this.value : ", this.value); // 1
        }
        // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는
        // 전역 객체가 바인딩된다.
        bar();
    }
}

obj.foo();
```

```js
// 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치하는 방법
var value = 1;

const obj = {
	value : 100,
    foo(){
        // this 바인딩(obj)을 변수 that에 할당
        const that = this;
        console.log("foo's this : ", this); // {value: 100, foo: ƒ}
        console.log("foo's this.value : ", this.value); // 100
        
        // 콜백 함수 내부에서 this 대신 that을 참조한다.
        function bar(){
            console.log("bar's that : ", that); // {value: 100, foo: ƒ}
            console.log("bar's that.value : ", that.value); // 100
        }
      
        bar();
    }
}

obj.foo();
```



<br>

<br>

### 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에

기술한 객체가 바인딩된다.

```js
const person = {
    name : 'Lee',
    getName(){
        return this.name;
    }
};

person.getName(); // Lee
```

메서드는 프로퍼티에 바인딩된 함수이며 위의 예제에서 person 객체의 getName 프로퍼티가 가리키는 함수 객체는

person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다. getName 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

<br>

즉 getName 프로퍼티가 가리키는 함수 객체, getName 메서드는 다른 객체의 프로퍼티에 할당하는 것과 다른 객체의

메서드가 될 수 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```js
const person = {
    name : 'Lee',
    getName(){
        return this.name;
    }
};

const anotherPerson = {
    name : 'Kim'
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
anotherPerson.getName() // 'Kim'

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
getName(); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
```

<br>

프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```js
function Person(name){
    this.name = name;
}

Person.prototype.getName = function(){
    return this.name;
}

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
me.getName(); // 'Lee'

Person.prototype.name = 'Kim';

Person.prototype.getName(); // Kim
```



<br>

<br>

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```js
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    }
}

const circle = new Circle(5);
circle.getDiameter(); // 10

// new 연산자와 함께 호출하지 않으면 생성자 함수가 아니라 일반적인 함수의 호출
const circle2 = Circle(10);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환
circle2 // undefined

```



<br>

<br>

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드이므로 모든 함수가 상속받아 사용할 수 있다.

```js
// Function.prototype.apply, Function.prototype.call 메서드는 this로 사용할 객체와 인수 리스트를
// 인수로 전달받아 함수를 호출한다.

/*
주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
@param thisArg - this로 사용할 객체
@param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
@returns 호출된 함수의 반환값
*/
Function.prototype.apply(thisArg[, argsArray])

/*
주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
@param thisArg - this로 사용할 객체
@param arg1, arg2, ... - 함수에게 전달할 인수 리스트
@returns 호출된 함수의 반환값
*/
Function.prototype.call (thisArg[, arg1[, arg2[, ...]]])
```

```js

function getThisBinding(){
    return this;
}

// this로 사용할 객체
const thisArg = { a: 3};

getThisBinding(); // window


// apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것
// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다
// getThisBinding 함수에 인수를 전달하지 않는다.
getThisBinding.apply(thisArg) // {a: 3}
getThisBinding.call(thisArg) // {a: 3}
```

```js
function getThisBinding(){
    console.log(arguments);
    return this;
}

const thisArg = { a: 3};

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
// getThisBinding 함수에 인수를 전달한다.
getThisBinding.apply(thisArg, [1,2,3]) // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
//{a: 3}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
getThisBinding.call(thisArg,1,2,3) // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
//{a: 3}

```

<br>

bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용

```js
const person = {
    name : 'Lee',
    foo(callback){
        // 콜백 함수 호출되기 시점의 this는 foo를 호출한 객체, person 객체
        setTimeout(callback, 100);
    }
};

person.foo(function(){
    console.log(`Hi! my name is ${this.name}.`) // Hi! my name is .
    // 일반 함수로 호출된 콜백 함수의 내부의 this.name은 브라우저 환경에서 window.name과 같다.
})
```

```js
// person.foo의 콜백함수는 외부함수 person.foo를 돕는 보조 함수 역할을 하기 때문에 외부 함수
// person.foo 내부의 this와 콜백 함수 내부의 this가 상이하면 문맥상 문제가 발생한다
// 콜백 함수 내부와 외부 함수 내부의 this를 일치시켜야 하는데 bind메서드를 사용하여 일치시킬 수 있다.
const person = {
    name : 'Lee',
    foo(callback){
        // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function(){
    console.log(`Hi! my name is ${this.name}.`) // Hi! my name is Lee.
})
```



---

<br><br>

<table>
    <caption>요약</caption>
    <th>함수 호출 방식</th>
    <th>this 바인딩</th>
    <tr><td>일반 함수 호출</td><td>전역 객체</td></tr>
    <tr><td>메서드 호출</td><td>메서드를 호출한 객체</td></tr>
    <tr><td>생성장 함수 호출</td><td>생성자 함수가 생성할 인스턴스</td></tr>
    <tr><td>Function.prototype.apply/call/bind 메서드에 의한 간접 호출</td><td>Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체</td></tr>
</table>

