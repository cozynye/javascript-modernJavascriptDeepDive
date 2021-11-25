

# 함수



## 6. 참조에 의한 전달과 외부 상태의 변경

원시 값은 값에 의한 전달, 객체는 참조에 의한 전달 방식으로 동작

매개변수도 함수 몸체 내부에서 변수와 동일하게 취급되므로 

매개변수도 타입에 따라(원시,객체) 값에 의한 전달, 참조에 의한 전달 방식을 따른다.

```js
//primitive는 원시 값을 전달 받고, obj는 객체를 전달 받음
function changeVal(primitive, obj){
    // 원시 타입 인수는 값 자체가 복사되어 매개변수에 전달 -> 원본값 훼손시키지 않음
    // 객체 타입 인수는 참조 값 복사되어 매개변수에 전달 -> 원본 변경 가능
    primitive+=100; // -> 원시 값은 재할당을 통해 할당된 원시 값은 새로운 원시 값으로 교체 why? 원시 값은 변경 불가능한값
    obj.name = 'Kim' // -> 객체는 변경 가능한 값이므로 직접 할당된 객체를 변경
}

var val = 100;
var person = { name : 'Lee' };

changeVal(val, person);

val // 100
person // {name: 'Kim'} -> 객체는 원본이 변경되었다.
```

-> 객체가 변경할 수 있는 값이며, 참조에 의한 전달 방식으로 동작하기 때문에 발생하는 부작용



이러한 문제의 해결 방법 객체를 원시 값처럼 변경 불가능한 값으로 동작하게 만드는 것이다. 

이를 통해 객체의 상태 변경 원천봉쇄하고 상태 변경이 필요한 경우 객체의 깊은 복사를 통해 

새로운 객체를 생성하고 재할당을 통해 교체한다.



<br>

## 7. 다양한 함수의 형태

#### 즉시 실행 함수

> 즉시 실행 함수(immediately invoked Function Expression) - 함수 정의와 동시에 즉시 호출되는 함수
>
> 한 번만 호출되며 다시 호출할 수 없다.

```js
// 즉시 실행함수는 이름 없는 익명 함수를 사용하는 것이 일반적
(function(){
    return 5*5;
}()); // 25

// 기명 즉시 실행함수에서 그룹 연산자() 내의 기명함수는 함수 선언문이 아니라 함수 리터럴로 평가되며 함수 이름은 함수 몸체에서만
// 참조할 수 있는 식별자이므로 즉시 실행 함수를 다시 호출할 수 없다.
(function a(){
    return 5*5;
}()); // 25
a() // ReferenceError: a is not defined
```



```js
// 즉시 실행 함수는 그룹 연산자()로 감싸야 한다.
function(){}; // SyntaxError: Function statements require a function name
// 함수 이름을 생략해 함수 정의가 함수 선언문의 형식에 맞지 않기 때문에 에러가 난다.

(); //SyntaxError: Unexpected token ')'
function foo(){}(); //SyntaxError: Unexpected token ')'
// 함수 선언문 뒤의 ()는 함수 호출 연산자가 아니라 그룹 연산자로 해석되고 그룹 연산자의 피연산자가 없기 때문에 에러가 발생

//그룹 연산자의 피연산자는 값으로 평가되므로 기명 또는 무명 함수를 그룹 연산자로 감싸면 함수 리터럴로 평가되어 함수 객체가 됨
(function(){}); // (function(){});
typeof (function(){}); // 'function'
```



```js
// 즉시 실행함수 사용 방법
(function(){
    console.log(1,2)
}()); // -> 가장 일반적인 방법

(function(){
    console.log(3,4)
})();

!function(){
    console.log(5,6)
}();

+function(){
    console.log(7,8)
}();
```



즉시 실행함수도 일반 함수처럼 값을 반환하거나 인수를 전달할 수 있다.

```js
var res = (function (){
    return '안녕하세요';
}());

res // '안녕하세요'

res = (function(a, b){
    return a * b;
}(3, 5));

res // 15
```



#### 재귀 함수

> 재귀 함수 - 자기 자신의 호출을 수행하는 함수

```js
function countdown(n){
    for(var i = n ; i >= 1 ; i--) console.log(i);
}
countdown(10); // 10 9 8 7 6 5 4 3 2 1

// 반복문 없이 재귀 함수를 사용해 구현 가능
function countdown(n){
    if(n === 0) return ;
    console.log(n);
    return countdown(n-1);
} // 10 9 8 7 6 5 4 3 2 1
```

재귀 함수는 자신을 무한 재귀 호출하므로 재귀 호출을 멈출 수 있는 **탈출 조건**을 반드시 만들어야 한다.



재귀 함수는 반복되는 처리를 반복문 없이 구현할 수 있는 장점이 있지만,

무한 반복에 빠질 위험이 있고 스택 오버플로 에러를 발생시킬 수 있으므로 

직관적으로 이해하기 쉬울 때만 한정적으로 사용하는 것이 바람직하다



#### 중첩 함수

> 중첩 함수, 내부 함수 - 함수 내부에 정의된 함수 
>
> -> 일반적으로 외부 함수를 돕는 헬퍼 함수의 역할을 한다.
>
> 외부 함수 - 중첩 함수를 포함하는 함수

중첩 함수는 외부 함수 **내부**에서만 호출 가능

```js
function outer(n){
    
    // 중첩함수 내부에서는 외부함수의 변수를 참조할 수 있다.
    function inner(){
        return n ** 2;
    }
    if(typeof n === 'number'){
    n = inner();
    return n;
    }else{
        return '숫자 타입이 아닙니다';
    }
}

outer(10) // 100
outer('야호') // '숫자 타입이 아닙니다'
```



#### 콜백 함수

> 콜백 함수(callback function) - 함수의 매개변수를 통해 다른 내부로 전달되는 함수
>
> 고차 함수(Higher-Order Function) - 매개 변수를 통해 함수의 외부에서 콜백 함수를 전달 받은 함수

함수의 변하지 않는 공통 로직은 미리 정의하고, 변경되는 로직은 추상화 해서 함수 외부에서 내부로 전달하면 새로운 함수를 만들 필요가 없다.

```js
function repeat(n,f){
    for ( var i = 0; i<n ; i++){
        f(i);
    }
}

var logAll = function(i){
    console.log(i)
};

var logOdds = function(i){
    if(i%2) console.log(i);
};

repeat(10,logAll); // 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
repeat(10,logOdds); // 1, 3, 5, 7, 9

// 콜백 함수가 고차 함수 내부에만 호출된다면 콜백 함수를 익명 함수 리터럴로 정의하면서 고차 함수에 전달하는 것이 일반적이다
repeat(10, function(i) { if(i % 2 ===0) console.log(i) }) // 0, 2, 4, 6, 8
```

1. 고차 함수는 매개 변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출한다.
2. 콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다.



#### 순수 함수와 비순수 함수

> 순수 함수 - 부수 효과가 없는 함수
>
> 비순수 함수 - 외부 상태에 의존하거나 외부상태를 변경하는, 즉 부수 효과가 있는 함수

순수 함수는 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수

```js
var count = 0;

function increase(n){
    return ++n;
}

//순수 함수가 반환한 결과를 변수에 재할당해서 상태를 변경
count = increase(count) 
count // 1
count = increase(count) 
count // 2
```



비 순수 함수는 외부 상태에 따라 반환값이 달라지는 함수이며 외부 상태에 의존하거나 외부 상태를 변경하는 함수다.

```js
var count = 0;

function increase(){
    return ++count; // 외부 상태를 변경
}

increase();
count // 1
increase();
count // 2
```

