# strict mode

## 1. strict mode란?

strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트

엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

```js
'use strict'; // 하지 않으면 자바스크립트는 x 변수를 암묵적으로 전역 변수처럼 사용할 수 있기 때문에 콘솔에 10이 찍힌다
function foo(){
    x = 10;
}
foo();
console.log(x); // x is not defined
```



<br>

<br>

## 2. strict mode의 적용

strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가한다.

```js
'use strict';
function foo(){
    x = 10;
}
```

```js
function foo(){
   'use strict'; 
    x = 10;
}
```



<br>

<br>

## 3. 전역에 strict mode를 적용하는 것을 피하자



```html
<!DOCTYPE html>
<html>
    <body>
        <script>
        	'use strict';
        </script>
        <script>
        	x = 1; // 에러가 발생하지 않는다
            console.log(x) // 1
        </script>
        <script>
        	'use strict';
            
            y = 1; // ReferenceError : y is not defined
            console.log(y);
        </script>
    </body>
</html>
```

위 예제와 같이 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용

하지만 strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있기 때문에

전역에 strict mode를 적용하는 것은 좋지 않다.

이러한 경우에는 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

```js
(function(){
    'use strict';
}())
```



<br>

<br>

## 4. 함수 단위로 strict mode를 적용하는 것도 피하자

어떤 함수는 strict mode를 적용하고 어떤 함수는 strict mode를 적용하지 않는 것은 적절하지 않다.

모든 함수에 strict mode를 적용하는 것은 번거로우며 strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에

strict mode를 적용하지 않는다면 문제가 생길 수 있다.

```js
(function(){
    var let = 10; // 에러가 발생하지 않는다.
    
    function foo(){
        'use strict';
        
        let = 20; //  SyntaxError: Unexpected strict mode reserved word
    }
    foo();
}())
```

따라서 strict mode는 즉시 실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.



<br>

<br>

## 5. strict mode가 발생시키는 에러

### 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다

```js
(function(){
    'use strict';
    
    x= 1;
    console.log(x); // ReferenceError: x is not defined
}());
```



<br>

### 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.

```js
(function(){
    'use strict';
    
    var x = 1;
    delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```



<br>

### 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.

```js
(function(){
    'use strict';
    
    function foo(x,x){ // SyntaxError: Duplicate parameter name not allowed in this context
        return x+ x ;
    }
}());
```



<br>

### with 문의 사용

with 문을 사용하면 SyntaxError가 발생한다.

with 문은 전달된 객체를 스코프 체인에 추가하는데 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서

코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다.

```js
(function(){
    'use strict';
    
    //SyntaxError: Strict mode code may not include a with statement
    with({ x : 1}){
        console.log(x)
    }
}());
```



<br>

<br>

## 6. strict mode 적용에 의한 변화

### 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다.

생성장 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다(에러는 발생하지 않는다.)

```js
(function(){
    'use strict';
    
    function foo(){
        console.log(this); // undefined
    }
    foo(); 
    
    function Foo(){
        console.log(this);  // Foo {}
    }
    new Foo();
}());
```



<br>

### arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```js
(function(a){
    console.log(arguments); // Arguments [1, callee: (...), Symbol(Symbol.iterator): ƒ]
     a = 2;
    console.log(arguments); // Arguments [2, callee: ƒ, Symbol(Symbol.iterator): ƒ]
}(1));

(function(a){
    'use strict';
    
    console.log(arguments); // Arguments [1, callee: (...), Symbol(Symbol.iterator): ƒ]
    
    // 매개변수에 전달된 인수를 재할당하여 변경
    a = 2;
    // 변경된 인수가 arguments 객체에 반영 되지 않는다.
    console.log(arguments); // Arguments [1, callee: (...), Symbol(Symbol.iterator): ƒ]
}(1));
```

