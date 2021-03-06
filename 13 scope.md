## 스코프



## 1. 스코프란?

> **스코프** - 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다. -> 식별자가 유효한 범위

> **식별자 결정** - 자바스크립트 엔진은 이름이 같은 변수중에서 어떤 변수를 참조해야 할 것인지를 결정해야 한다.

```js
var x = 'global';

function foo(){
    var x = 'local';
    console.log(x)//①
}
foo();
console.log(x);//②
```

위의 예제에서 코드 가장 바깥 영역의 변수 x는 어디서든 참조할 수 있지만 foo 함수 내부에서 선언된 변수 x는 foo 함수 내부

에서만 참조할 수 있고 외부에서는 참조할 수 없다.

-> 두 개의 x 변수는 식별자 이름은 동일하지만 **스코프가 다른 별개의 변수**이다.



프로그래밍 언어에서는 스코프를 통해 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있게 한다.

스코프 내에서 식별자는 유일해야하지만 다른 스코프에는 같은 이름의 식별자를 사용할 수 있다.



```js
function foo(){
    var x = 1;
    var x = 2; // var 키워드로 선언된 변수는 같은 스코프 내 중복 선언 허용
   // -> 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하기 때문
    
    console.log(x); // 2
}

function bar(){
    let x = 1;
    let x = 2; // SyntaxError: Identifier 'x' has already been declared
    // let이나 const 키워드로 선언된 변수는 같은 스코프 내 중복 선언 X
    console.log(x);
}
```



<br>

## 2. 스코프의 종류

변수는 자신이 선언된 위치(전역 또는 지역)에 의해 자신이 유효한 범위인 스코프가 결정된다.

<table>
    <th>구분</th>
    <th>설명</th>
    <th>스코프</th>
    <th>변수</th>
    <tr><td>전역</td><td>코드의 가장 바깥 영역</td><td>전역 스코프</td><td>전역 변수</td></tr>
    <tr><td>지역</td><td>함수 몸체 내부</td><td>지역 스코프</td><td>지역 변수</td></tr>
</table>



#### 전역, 전역 스코프, 지역, 지역 스코프

> 전역 - 코드의 가장 바깥 영역. 전역은 전역 스코프를 만든다.
>
> 전역에 변수를 선언하면 전역 스코프를 갖는 전연 변수가 된다.

> 지역 - 함수 몸체 내부
>
> 지역은 지역 스코프를 만든다. 지역에 변수를 선언하면 지역 스코프를 갖는 지역 변수가 된다.
>
> 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

```js
var x = 'global x';
var y = 'global y';

function outer(){
    var z = "outer's local z"; //-> 자신의 지역 스코프인 outer 함수와 내부 하위 지역 스코프인 inner 함수 내부에서만 참조가능
    
    console.log(x); //global x
    console.log(y); //global y
    console.log(z); //outer's local z
    
    function inner(){
        var x = "inner's local x";
        
        console.log(x); //inner's local x
        //-> 지역 변수 inner의 x 와 전역 변수 x 가 있는데 지역 변수 x를 참조한 것은 자바스크립트 엔진이 스코프 체인을 통해 참			조할 변수를 검색했기 때문
        console.log(y); //global y
        console.log(z); //outer's local z
    }
    
    inner();
}
outer();

console.log(x); //global x
console.log(z); //ReferenceError: z is not defined
```



<br>

## 3. 스코프 체인

함수는 전역에서 정의할 수도, 함수 몸체 내부에서도 정의할 수 있다.

> **함수의 중첩** - 함수 몸체 내부에서 함수가 정의된 것
>
> **중첩 함수** - 함수 몸체 내부에서 정의한 함수
>
> **외부함수** - 중첩 함수를 포함하는 함수



함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있다.

-> 스코프가 함수의 중첩에 의해 계층적 구조를 갖는다는 의미

<img src=./image/scope1.png height=400>

그림처럼 모든 스코프는 하나의 계층적 구조로 연결되며 , 모든 지역 스코프의 최상위 스코프는 전역 스코프다.

> **스코프 체인** - 스코프가 계층적으로 연결된 것

변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 **변수를 참조하는 코드의 스코프에서**

**시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색**한다.



#### 스코프 체인에 의한 변수 검색

위 예제의 inner 함수를 살펴보자.

```js
  function inner(){
        var x = "inner's local x";
        
      // x 변수를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서 x 변수가 선언되었는지 검색
      // -> inner 함수 내에 선언된 x 변수 가 존재 -> 검색된 변수를 참조하고 검색을 종료
        console.log(x);
      
       // y 변수를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서 y 변수가 선언되었는지 검색
      //-> inner 함수 내에는 y 변수의 선언 존재 X -> 상위 스코프인 outer 함수의 지역 스코프로 이동
      //-> outer 함수 내 y 변수 선언 존재 X -> 상위 스코프인 전역 스코프로 이동
      //-> 전역 스코프에는 y 변수의 선언이 존재 -> 검색된 변수를 참조하고 검색을 종료
        console.log(y); 
      
       // z 변수를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서 z 변수가 선언되었는지 검색
      //-> inner 함수 내에는 z 변수의 선언 존재 X -> 상위 스코프인 outer 함수의 지역 스코프로 이동
      // -> outer 함수 내 z 변수 선언 존재  -> 검색된 변수를 참조하고 검색을 종료
        console.log(z); 
    }
```

위처럼 자바스크립트 엔진은 스코프 체인을 따라 변수를 참조하는 코드의 스코프에서 시작해 

상위 스코프 방향으로 이동하며 선언된 변수를 검색한다.(하위 스코프로 내려가면서 식별자 검색 X)

```js
function foo(){
    console.log('global function');
}

function bar(){
    function foo(){
        console.log('local function');
    }
    foo();
}

bar(); // local function
//-> 함수도 변수와 마찬가지로 스코프를 갖기 때문
```



-> **상위 스코프에서 유효한 변수는 하위 스코프에서 참조 가능하지만 하위 스코프에서 유효한 변수를 상위 스코프에서는 참조 할 수 없다.**

<br>

## 4. 함수 레벨 스코프



지역은 함수 몸체 내부를 말하고 지역은 지역 스코프를 만든다.

-> 코드 블록이 아닌 **함수에 의해서만 지역 스코프가 생긴다**.

var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정

-> 이러한 특성을 **함수 레벨 스코프**라 한다.

> **블록 레벨 스코프** - 함수 몸체 뿐만 아니라 모든 코드 블록(if, for while, try/catch 등)이 지역 스코프를 만든다.
>
> ES6에서 도입된 let, const 키워드는 블록 레벨 스코프를 지원

```js
var x = 1;
if(true){
    var x = 10;
}

x // 10;
//-> var 키워드로 선언된 변수는 함수의 코드 블록(함수 몸체)만은 지역 스코프로 인정
//-> 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되어도 모두 전역 변수!
// 블록 내의 x도 전역 변수다 이미 밖의 전역 변수 x 가 있으므로 x 변수는 중복 선언 되고 
// 값을 변경시키는 부작용을 발생시킨다.
```



## 5. 렉시컬 스코프

> **렉시컬 스코프**(lexical scope) - 함수를 어디서 정의했는지에 따라 함수의상위 스코프를 결정한다.
>
> ->**정적 스코프**라고도 한다.

```js
var x = 1;

function foo(){
    var x = 10;
    bar();
}

// 함수의 상위 스코프는 언제나 자신이 정의된 스코프
function bar(){ // bar는 전역에서 정의된 함수
    console.log(x);
}

foo();
bar();
```

자바스크립트는 함수를 **어디서 정의했는지에 따라 상위 스코프를 결정**한다.

-> 함수가 호출된 위치는 상위 스코프 결정에 영향을 주지 않는다.



----

함수의 상위 스코프는 함수 정의가 실행 될 때 정적으로 결정된다.

함수 정의(함수 선언문 또는 함수 표현식)가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다.

함수가 호출될 때 마다 함수의상위 스코프를 참조할 필요가 있기 때문이다.

----



위 예제의 bar 함수는 전역에서 정의된 함수 -> 함수 선언문으로 정의된 bar 함수는 전역 코드가 실행되기

전에 먼저 평가되어 함수 객체를 생성 -> bar 함수는 자신이 정의된 스코프, 전역 스코프를 기억 ->

bar 함수가 호출되면 호출된 곳 상관없이 자신이 기억하고 있는 전역 스코프를 상위 스코프로 사용