# 함수

## 1. 함수란?

> **함수** - 일련의 과정을 문(statement)으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것
>
> 매개변수(parameter) - 함수 내부로 입력을 전달받는 변수
>
> 인수(argument) - 입력 값
>
> 반환값(return value) - 함수의 출력 값
>
> 함수 이름 - 특정 함수를 구별하기 위한 식별자

<br>

## 2. 함수를 사용하는 이유

1. 코드의 재사용 - 함수는 몇 번이든 호출할 수 있음.
2. 유지보수의 편의성 - 코드의 중복을 억제하고 재사용성을 높임.
3. 코드의 가독성 - 적절한 함수 이름은 코드를 이해하지 않더라도 함수의 역할을 파악할 수 있도록 함.

<br>

## 3. 함수 리터럴

> 리터럴 - 값을 생성하기 위한 표기법

자바스크립트의 함수는 객체 타입의 값.(함수 리터럴도 평가되어 값을 생성하며 이 값은 객체)

함수는 function 키워드, 함수 이름, 매개 변수 목록, 함수몸체로 구성된 함수 리터럴로 생성할 수 있다.

```js
var f = function add(x, y){
    return x + y;
};
```

<table>
    <caption style="caption-side:bottom; padding : 10px">함수 리터럴의 구성 요소</caption>
    <th>구성 요소</th>
    <th>설명</th>
    <tr>
        <td>함수 이름</td>
    	<td>- 함수 이름은 식별자 -> 식별자 네이밍 규칙 준수<br>
        	- 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자<br>
            - 함수 이름은 생략 가능(기명함수 - 이름이 있는 함수 / 무명,익명함수 - 이름이 없는 함수<br>
        </td>
    </tr>
    <tr>
        <td>매개변수 목록</td>
    	<td>- 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분<br>
        	- 매개변수에는 함수를 호출할 때 지정한 인수가 <strong>순서</strong>대로 할당<br>
            - 매개변수는 함수 몸체 내에서 변수와 동일 취급 -> 식별자 네이밍 규칙 준수
        </td>
    </tr>
<tr>
        <td>함수 몸체</td>
    	<td>- 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록<br>
        	- 함수 몸체는 함수 호출에 의해 실행<br>
        </td>
    </tr>
</table>

-----

일반 객체는 호출 X, 함수는 호출 O

---

<br>



## 4. 함수 정의

> 함수 정의 - 함수를 호출하기 전에 인수를 전달받을 매개변수와 실행할 문들, 반환할 값을 지정하는 것
>
> -> 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 됨.

<table>
    <th>함수 정의 방식</th>
    <th>예시</th>
    <tr>
    	<td>
            함수 선언문
        </td>
        <td>
            function add(x, y){
            	return x+y;
            }
        </td>
    </tr>
    <tr>
    	<td>
            함수 표현식
        </td>
        <td>
            var add =function(x, y){
            return x+y;
           	}
        </td>
    </tr>
    <tr>
    	<td>
            Function 생성자 함수
        </td>
        <td>
            var add = New function('x', 'y', 'return x + y')
        </td>
    </tr>
    <tr>
    	<td>
            화살표 함수(ES6)
        </td>
        <td>
            var add = (x, y) => x + y;
        </td>
    </tr>
</table>





#### 함수 선언문

함수 선언문은 함수 리터럴과 형태가 동일하다. 

함수 리터럴은 함수 이름 생략 가능하지만 함수 선언문은 함수 이름을 생략할 수 없다.

```js
function add(x, y){
    return x + y;
}
```



함수 선언문은 **표현식이 아닌 문**이다 -> 변수에 할당 불가

```js
//함수 선언문이 변수에 할당되는 것처럼 보인다.
var add = function add(x, y){
    return x + y;
}

add(2,5) // 7
```

-----

이렇게 동작하는 이유

-> 자바스크립트 엔진이 

코드의 문맥에 따라 함수리터럴을 표현식이 아닌 문인 함수 선언문으로 해석하는 경우와

표현식인 문인 함수 리터럴 표현식으로 해석하는 경우가 있기 때문이다.

----

자바스크립트 엔진은 

함수 이름이 있는 함수 리터럴을 단독으로 사용(함수 리터럴을 피연산자로 사용하지 않는 경우)하면 함수 선언문으로 해석

함수 리터럴이 값으로 평가되어야 하는 문맥(함수 리터럴을 변수에 할당, 피연산자로 사용)에서는 함수 리터럴 표현식으로 해석

------------

어떤 방식이든 함수가 생성되는 것은 동일하지만 생성하는 내부 동작에 차이가 있다.

```js
// 기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석
function foo(){console.log("foo")}
foo() // foo

// 함수 리터럴을 피연산자로 사용하면 함수 리터럴 표현식으로 해석
(function bar(){console.log("bar")}) // 그룹 연산자()의 피연산자는 값으로 평가 될 수 있는 표현식 이여야 한다.
foo() // ReferenceError: foo is not defined
```

함수 리터럴에서 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자

-> 함수 몸체 외부에서는 함수이름으로 함수를 참조할 수 없으므로 외부에서는 함수 이름으로 함수를 호출할 수 없다.

-> 함수를 가리키는 식별자가 없으므로 위 예제의 bar 함수는 호출할 수 없다.

<img src=./image/function1.png height=400>





foo() 호출 가능한 이유 ->

자바스크립트 엔진은 함수 선언문을 해석해 함수 객체를 생성한다. 

함수 이름은 함수 몸체 내부에서만 유효한 식별자이므로 함수 이름과는 별도로 생성된 함수 객체를 가리키는 식별자가 필요한데 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 **동일한 이름의 식별자를 암묵적으로 생성**, 거기에 함수 객체를 할당

<img src=./image/function2.png height=400>

----

함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출

위의 예제로 본다면 함수 이름 foo가 아니라 자바스크립트 엔진이 암묵적으로 생성한 식별자 foo이다.

-------------





#### 함수 표현식

자바스크립트의 함수는 **일급 객체**다. 

> 일급 객체 - 변수에 할당할 수 있고, 프로퍼티의 값, 배열의 요소가 될 수 있는 값의 성질을 갖는 객체



함수 리터럴로 생성한 함수 객체를 변수에 할당하는 방식을 **함수 표현식**이라고 한다.

```js
// 함수 리터럴에서 함수 이름 sum은 생략 가능하고 생략하는 것이 일반적이다.
var add = function sum(x, y){
    return x + y;
};

add(3,5) // 8
sum(3,5) //ReferenceError: sum is not defined
```

-----

함수 선언문은 "표현식이 아닌 문" / 함수 표현식은 "표현식인 문"

->유사하게 동작하는 것처럼 보이지만 동일하게 동작하지는 않는다

-----

#### 함수 생성 시점과 함수 호이스팅

> console.dir은 console.log와 달리 함수 객체의 프로퍼티까지 출력
>
> <img src=./image/function3.png height=200>



```js
// 함수 참조
console.dir(add); // ƒ add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2,5)); // 7
console.log(sub(2,5)); // TypeError: sub is not a function

function add(x, y){
    return x + y;
}

var sub = function(x, y){
    return x - y;
};

// -> 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출 가능하지만 함수 표현식은 호출 할 수 없다.
// -> 함수 선언문과 함수 표현식의 생성 시점이 다르기 때문
```

> 런타임 - 코드가 한줄 씩 순차적으로 실행되는 시점

함수 선언문으로 함수를 정의하면 런타임 이전에 함수 객체가 먼저 생성 -> 식별자를 암묵적으로 생성하고 생성된 함수 객체를 할당

-> 코드가 실행되기 시작하는 런타임에는 이미 함수 객체 생성되어 있고 식별자에 할당까지 끝난 상태

> 함수 호이스팅(function hoisting) - 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징

---

변수 호이스팅인 var 키워드로 선언된 변수는 undefined로 초기화 되고, 함수 선언문을 통해 생성된 식별자는 함수 객체로 초기화된다.

-> 즉 함수 표현식은 변수에 할당되는 값이 함수 리터럴인 문이기에 **변수 선언은 이전에 실행되어 undefined로 초기화** 되지만 **변수 할당문의 값은 런타임에 평가**되므로 함수 표현식의 함수 리터럴 문도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.

-> 함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생한다.

> **변수 호이스팅** - 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징

위의 예를 보면 할당 전에 호출하면 undefined를 호출하는 것과 마찬가지이므로 타입에러가 발생



#### 생성자 함수

```js
var add = new Function('x', 'y', 'return x + y');
add //7
```

생성자 함수로 생성한 함수는 클로저closure)를 생성하지 않고 함수 선언문이나 표현식으로 생성한 함수와 다르게 동작하는 등 일반적이지 않고 바람직하지 않음

> 클로저(closure) - 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것

```js
var add1 = (function(){
    var a = 10;
    return function(x, y){
        return x + y + a;
    };
}());
add1(1,2) // 13

var add2 = (function(){
    var a = 10;
    return new Function('x', 'y', 'return x + y + a;');
}());

add2(1,2) // ReferenceError: a is not defined
```



#### 화살표 함수

화살표 함수는 function 키워드 대신 화살표 => 를 사용해 간략한 방법으로 함수를 선언할 수 있다.(화살표 함수는 항상 익명 함수로 정의)

```js
const add = (x, y) => x + y;
add(1,2) // 3
```

화살표 함수는 생성자 함수로 사용할 수 없고, 기존 함수와 this 바인딩이 다르고, prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않는다.



<br>



## 5. 함수 호출

함수는 함수를 가리키는 식별자와 한 쌍의 소괄호인 함수 호출 연산자로 호출

함수를 호출하면 현재의 실행 흐름을 중단하고 호출된 함수로 실행 흐름을 옮긴다.



#### 매개변수와 인수

함수를 실행하기 위해 필요한 값을 함수 외부에서 함수 내부로 전달할 필요가 있는 경우 매개변수(인자)를 통해 인수를 전달한다.

인수는 값으로 평가될 수 있는 표현식이어야 한다.

함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 생성되고 일반 변수와 마찬가지로 undefined로 초기화 된 후 인수가 순서대로 할당된다.

```js
function add(x, y){
    return x + y;
}

// 매개변수 x, y 는 함수 몸체 내부에서만 참조 가능
add(x, y) // ReferenceError: x is not defined

// 매개변수 x에는 인수 1이 전달되지만 y에는 전달할 인수가 없으므로 undefined로 초기화된 상태 그대로이다.
add(1) // NaN -> 1 + undefined
add(1,2,3) // 3 // 매개변수보다 인수가 많은 경우 초과된 인수는 무시된다.
```



#### 인수 확인

1. 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
2. 자바스크립트는 동적 타입 언어이므로 매개변수 타입을 사전에 지정할 수 없다.

```js
// typeof를 사용해 적절한 타입인지 확인하는 방법
function add(x, y){
    if(typeof x !== 'number' || typeof y !== 'number'){
        throw new TypeError('인수가 숫자 값이 아닙니다.')
    }
    return x + y;
}

add(3) //TypeError: 인수가 숫자 값이 아닙니다.
add(2,'b') //TypeError: 인수가 숫자 값이 아닙니다.

// 단축 평가를 통해 인수가 전달되지 않은 경우 매개변수에 기본 값 설정 방법
function sum(x, y, z){
    a = x || 0;
    b = y || 0;
    c = z || 0;
    return a + b + c;
}

sum(1, 2, 3) // 6
sum(1, 2) // 3
sum(1) // 1
sum(0) // 0

//ES6에 도입된 매개변수 기본값을 사용하면 인수 체크 및 초기화를 간소화 할 수 있음
// 기본값은 인수를 전달하지 않았을 경우와 undefined를 전달한 경우에만 유효
function sub(x = 0, y = 0, z = 0){
    return x - y - z;
}
sub(10,5,2) // 3
sub(10,5) // 5
sub(10) // 10
sub('a') // NaN
sub('a','b') // NaN
```



#### 매개변수의 최대 개수

매개 변수의 최대 개수에 대해 명시적으로 제한하고 있지만 인수가 많을 수록 함수의 사용법을 이해하기 어렵고 실수를 발생시킬 가능성을 높이므로 이상적인 매개변수의 개수는 0개이며 적을수록 좋다.

매개변수는 최대 3개 이상을 넘지 않는 것을 권장하며 그 이상의 매개변수가 필요하다면 하나의 매개변수를 선언하고 객체를 인수로 전달하는 것이 유리하다.

```js
//jQuery의 ajax 메서드에 객체를 인수로 전달하는 예

$.ajax({
    method: 'POST',
    url: '/user',
    data: { id: 'hong', name : 'chu'},
    cache:false
});
// -> 객체를 인수로 사용하는 경우 프로퍼티 키만 정확히 지정하면 매개변수의 순서를 신경 쓰지 않아도 되고 명시적으로 인수의 의미를 설명하는 프로퍼티 키를 사용하게 되므로 코드의 가독성도 좋아지고 실수도 줄어드는 효과가 있다.
```



#### 반환문

1. 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나간다. -> 반환문 이후에 다른 문은 실행되지 않는다
2. 반환문은 return 키워드 뒤에 오는 표현식을 평가해 반환한다.

```js
// return 뒤 표현식을 명시하지 않으면 undefined가 반환
function apple(){
    return;
}
apple() // undefined

// 반환문을 생략하면 암묵적으로 undefined가 반환
function banana(){
}
banana() // undefined

// return 키워드와 반환값으로 사용할 표현식 사이에 줄바꿈이 있을 경우 세미콜론 자동 삽입 기능에 의해 의도치 않은 결과가 발생함
function grape(){
    return
    '포도다'
}
grape() // undefined
```



