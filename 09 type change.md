# 타입 변환과 단축 평가

## 1\. 타입 변환이란?

기존 원시 값을 사용해 다른 타입의 **새로운 원시 값**을 생성하는 것

> **명시적 타입 변환explicit coercion(타입 캐스팅type casting)** - 개발자의 의도에 따라 다른 타입으로 변환

```
var num = 3;

//숫자를 문자열로 타입 변환 -> 기존의 변수 값의 변화는 주지 않음
var toString = num.toString();
```

> **암묵적 타입변환**implicit coercion(**타입 강제 변환**type coercion) - 표현식 평가 도중 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환

```
var num = 10;

//기존의 변수 값 변경X
var toString = num + '';
```

## 2\. 암묵적 타입 변환

#### 문자열 타입으로 변환

+연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작(피연산자 중 문자열이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환

```
1 + '2' // '12'
```

템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암뭄적 타입 변환

```
`1 + 1 = ${1 + 1}`    // "1 + 1 = 2"
```

---

```
// 헷갈리기 쉬운 문자열 타입으로의 암묵적 변환

-0 + '' // "0"
-Infinity + '' // 'Infinity'
(Symbol()) + '' // TypeError : Cannot convert a Symbol value to a string
Array + '' // 'function Array() { [native code] }'
```

#### 숫자 타입으로 변환

자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 피연산자 중 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환

```
1 + '1' // '11' 주의 할 것
1 - '1' // 0
1 * '10' // 10
1/'one' // NaN

// 비교 연산자는 불리언 값을 만들기 위해 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환
'1' > 0 // true
```

\+ 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행

```
+ '' //0
+ 'string'
+ null // 0
+ undefined // NaN
+ Symbol() // TypeError : Cannot convert a Symbol value to a number
+ {} // NaN
+ [] // 0
+ [10,20] // NaN
+ (function(){}) // NaN
```

#### 불리언 타입으로 변환

자바스크립트 엔진은 논리적 참/거짓으로 평가되어야 하는 표현식일 때 불리언 값으로 평가 되어야 할 문맥에서 **Truthy 값**(참으로 평가되는 값)은 true로 **Falsy 값**(거짓으로 평가되는 값)은 false로 암묵적 타입 변환

```
if ('') console.log('1');
if (true) console.log('2');
if (0) console.log('3');
if ('abcd') console.log('4');
if (null) console.log('5');
// 2, 4
```

\*_false, undefined, null, 0, -0, NaN, \*_은 Falsy 한 값이다.(Falsy 값 제외 모든 값은 Truthy 값)

## 3\. 명시적 타입 변환

#### 문자열 타입으로 변환

1.  String 생성자 함수를 new 연산자 없이 호출하는 방법

```
String(NaN); // 'NaN'
String(true); // 'true'
```

2.  Object.prototype.toString 메서드를 사용하는 방법

```
(1).toString(); // '1'
(Infinity).toString(); // 'Infinity'
```

3.  문자열 연결 연산자를 이용하는 방법

```
1 + '' // '1'
true + '' // 'true'
```

#### 숫자 타입으로 변환

1.  Number 생성자 함수를 new 연산자 없이 호출하는 방법

```
Number('-1'); // -1
Number(false); // 0
```

2.  parseInt, parseFloat 함수를 사용하는 방법(문자열만 가능)

```
parseInt('-1'); // -1
parseInt('15.23') // 15.23
```

3.  \+ 단항 산술 연산자를 이용하는 방법

```
+'0'; // 0
+false; // 0
```

4.  \* 산술 연산자를 이용하는 방법

```
'-1' * 1; // -1
true * 1; // 1
```

#### 불리언 타입으로 변환

1.  Boolean 생성자 함수를 New 연산자 없이 호출하는 방법

```
Boolean(NaN); // false
Boolean(Infinity); // true
Boolean(null); // false
Boolean([]); // true
Boolean({}); // true
```

2.  ! 부정 논리 연산자를 두 번 사용하는 방법

```
!!'x';// true
!!''; //false
!!null; // false
!!{}; // true
```

## 4\. 단축 평가

> 단축 평가 - 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것

#### 논리 연산자를 사용한 단축 평가

논리합(||) 또는 논리곱(&&) 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

단축 평가 표현식평가 결과

| true \|\| anything  | true     |
| ------------------- | -------- |
| false \|\| anything | anything |
| true && anything    | anything |
| false && anything   | false    |

```
'Cat' || 'Dog' // 'Cat'
false || 'Dog' // 'Dog'
'Cat' || false // 'Cat'

'Cat' && 'Dog' // 'Dog'
false && 'Dog' // false
'Cat' && false // false
```

위의 예제처럼 논리곱 연산자와 논리합 연산자는 피연산자를 타입 변환하지 않고 그대로 반환

```
//객체를 가리키는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티 참조할 때 단축평가 사용하면 에러 발생 X
var fruits=null;
var apple=fruits.apple; // TypeError : Cannot read property of null

var banana = fruits && fruits.banana; //undefined

fruits={grape : 25};
var grape = fruits && fruits.grape; //25
```

#### 옵셔널 체이닝 연산자

> **옵셔널 체이닝**(optional chaining) - 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환, 아니면 우항의 프로퍼티 참조를 이어감

```
var sports = null;
var soccer = sports?.soccer;//undefined
soccer;//undefined

sports = {soccer:11}; //{soccer: 11}
soccer = sports?.soccer // 11
```

#### null 병합 연산자

null 병합(bullish coalescing) 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 아니면 좌항의 피연산자를 반환

```
var capital = null ?? "korea seoul"; 
capital // korea seoul

//좌항의 연산자가 Falsy한 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 반환
var capital = false ?? "korea seoul"; 
capital // false
```