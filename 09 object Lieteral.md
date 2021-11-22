# 객체 리터럴

## 1. 객체란?

자바스크립트는 객첵디반의 스크립트 언어이며 자바스크립트를 이루고 있는 거의 모든 것은 객체이다.

> **객체** - 관련된 데이터와 함수(프로퍼티, 메소드)의 집합.
>
> 여러 속성(프로퍼티)을 하나의 변수에 저장할 수 있도록 해주며 key/value의 구조로 저장됨.
>
> 함수도 프로퍼티 값으로 사용됨(일반 함수와 구분하기 위해 메서드method라 부름) -> 메서드는 객체에 묶여 있는 함수를 의미

```js
var counter = {
    num : 1,
    increase : function(){
        this.num++;//this는 counter 객체를 가리킴
    }
}
```

<br>

## 2. 객체 리터럴에 의한 객체 생성

> **프로토타입**(prototype) - 자바스크립트의 모든 객체는 프로토타입이라는 객체를 가지고 있다.
>
> 모든 객체는 그들의 프로토타입으로부터 프로퍼티와 메소드를 상속받는다.
>
> 자바스크립트의 모든 객체는 최소 하나 이상의 다른 객체로부터 상속을 받으며, 상속되는 정보를 제공하는 객체를 프로토타입이라 한다.
>
> 프로토 타입 특징 - 원본 객체가 존재하고 그 객체를 복제해서 새로운 객체를 생성하는 방법

자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 객체 방법을 지원

1. **객체 리터럴**(객체를 생성하기 위한 표기법) - 중괄호({...}) 내에 0개 이상의 프로퍼티를 정의
2. Object 생성자 함수
3. 생성자 함수
4. Object.create 메서드
5. 클래스(ES6)

> 리터럴 - 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기볍

가장 일반적으로 쓰이는 방법은 객체 리터럴을 사용하는 방법이다.

```js
var sports = {
    soccer : 11,
    play : function(){
      console.log(`축구는 ${this.soccer}명이서 하는 스포츠다. `);
    }
}; // 객체 리터럴은 값으로 평가되는 표현식이므로 중괄호는 코드블록을 의미하는 것이 아님 -> 중괄호 붙여야함
```



<br>

## 3. 프로퍼티

객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.

> 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
>
> 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

\* 프로퍼티 키는 식별자 네이밍 규칙을 따르는 경우 따옴표 생략 가능. 그렇지 않으면 반드시 따옴표 사용

```js
var person={
    firstName : 'gil-dong',
    'last-name' : 'Hong',
    middle-name : 'kaizer' //SyntaxError : Unexpected token '-'
    //-> middle-name을 -연산자가 있는 표현식으로 해석하기 때문에 따옴표를 붙여야함
};
```



문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.

```js
var father = 'father'
var person={
    firstName : 'gil-dong',
    'last-name' : 'Hong',
    'middle-name' : 'kaizer' 
};
person[father] = 'Hong ka';
person['mother'] = 'unknown'
```



프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.  

var, function 같은 예약어를 사용해도 에러는 나지 않으나 권장하지 않는다.

프로퍼티 키를 중복 선언시 나중에 선언한 프로퍼티 값으로 덮어쓴다.

```js
var num = {
    1 : 1,	// num.1 X -> num['1']
    2 : 3,
    var : '',
    function : function(){},
    var : 'tuple'
}
```

<br>

## 4. 프로퍼티 접근

프로퍼티에 접근하는 방법

1. 마침표 프로퍼티 접근 연산자(.)를 사용하는 **마침표 표기법**dot notation
2. 대괄호 프로퍼티 접근 연산자([ ... ])를 사용하는 **대괄호 표기법**bracket notation

두개의 접근 연산자 좌측에는 객체로 평가되는 표현식을 기술하고 접근 연산자 우측, 내부에는 프로퍼티 키를 지정

대괄호 프로퍼티 접근 연산자를 사용 할 때 프로퍼티 키는 따옴표로 감싼 문자열이여야 한다.(숫자로 이뤄진 문자열일 경우 생략 가능)

```js
var person={
    firstName : 'gil-dong',
    'last-name' : 'Hong',
    'middle-name' : 'kaizer',
    010 : 1111
};
console.log(person.firstName);
console.log(person['firstName']);
console.log(person[010]);
console.log(person[fullName]); //ReferenceError: fullName is not defined
							  // 식별자 fullName을 평가하기 위해 선언된 fullName 찾았지만 찾지 못함
console.log(person.notproperty); // 객체에 존재하지 않는 프로퍼티 접근하면 undefined를 반환
```

프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름이면 반드시 대괄호 표기법을 사용

```js
var person={
    firstName : 'gil-dong',
    'last-name' : 'Hong',
    'middle-name' : 'kaizer',
    010 : 1111
};
console.log(person.last-name); // person.last -> undefined 이니 undefined - name 으로 해석된다.
console.log(person.010); //SyntaxError: Unexpected number
```



<br>



## 5. 프로퍼티 값 갱신, 생성, 삭제

이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 **갱신** 됨.

존재하니 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 **생성**된다.

delete 연산자는 객체의 프로퍼티를 **삭제**한다.

```js
var rule = {
    del : 'delete',
    image : 'root/image'
};
rule.image = 'root/'; // 갱신
rule.rule = 'study'; // 생성

delete rule.del; // 삭제
delete rule.study; // 객체에 없는 프로퍼티를 삭제해도 에러가 발생하지 않는다.
```



<br>



## 6.  ES6에서 추가된 객체 리터럴의 확장 기능



#### 프로퍼티 축약 표현

객체 리터럴의 프로퍼티 값은 변수에 할당된 값, 즉 식별자 표현식일 수도 있다.

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략 할 수 있다.

키값은 변수 이름으로 자동 생성된다.

```js
// ES5
let x = 1, y = 2;
const obj = {x,y}; //{x: 1, y: 2}
```



#### 계산된 프로퍼티 이름

프로퍼티 키로 사용할 표현식을 대괄호로 묶어 프로퍼티 키를 동적으로 생성할 수 있다.

```js
//ES5
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

obj //{prop-1: 1, prop-2: 2, prop-3: 3}
```

```js
//ES6
const prefix = 'prop';
let i = 0;

const obj = {
    [`$[prefix]-${++i}`] : i,
    [`$[prefix]-${++i}`] : i,
    [`$[prefix]-${++i}`] : i
};

obj //{prop-1: 1, prop-2: 2, prop-3: 3} 이렇게 나와야 하는데
//{$[prefix]-1: 1, $[prefix]-2: 2, $[prefix]-3: 3} 나옴 더 살펴보자
```



#### 메서드 축약 표현

메서드 정의 할 때 function 키워드를 생략 가능

```js
const Meeting ={
    day: '2021-11-22',
    say(){
        console.log('Hi');
    }
};
```

