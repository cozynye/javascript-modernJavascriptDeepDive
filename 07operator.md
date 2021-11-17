# 연산자



> 연산자 - 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행해 하나의 값을 만듦.
>
> 이때 연산의 대상을 피연산자(값으로 평가될 수 있는 포현식)이라 함.

<br>

## 1. 산술 연산자

> **산술 연산자**(arithmetic operator) - 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만듦.
>
> 산술 연산지 불가능한 경우 NaN을 반환.



<br>

#### 1. 이항 산술 연산자

> **이항(binary) 산술 연산자** - 2개의 피연산자를 산술 연산하여 숫자 값을 만듦.

이항 산술 연산자는 피연산자의 값을 변경하는 부수 효과 없이 새로운 값을 만든다.

<table>
    <th width="150">이항 산술 연산자</th>
     <th width="150">의미</th>
     <th >부수 효과</th>
    <tr>
        <td>+</td>
        <td>덧셈</td>
        <td>X</td>
    </tr>
    <tr>
        <td>-</td>
        <td>뺼셈</td>
        <td>X</td>
    </tr>
    <tr>
        <td>*</td>
        <td>곱셈</td>
        <td>X</td>
    </tr>
    <tr>
        <td>/</td>
        <td>나눗셈</td>
        <td>X</td>
    </tr>
    <tr>
        <td>%</td>
        <td>나머지</td>
        <td>X</td>
    </tr>
</table>

<br>



#### 2. 단항 산술 연산자

<table>
    <th width="150">단항 산술 연산자</th>
     <th width="450">의미</th>
     <th width="150">부수 효과</th>
    <tr>
        <td>++</td>
        <td>증가</td>
        <td>O</td>
    </tr>
    <tr>
        <td>--</td>
        <td>감소</td>
        <td>O</td>
    </tr>
    <tr>
        <td>+</td>
        <td>어떠한 효과도 없다. 음수를 양수로 반전하지도 않는다.</td>
        <td>X</td>
    </tr>
    <tr>
        <td>-</td>
        <td>양수를 음수로, 음수를 양수로 반전한 값을 반환한다.</td>
        <td>X</td>
    </tr>
</table>

```javascript
//++, -- 연산자는 피연산자의 값을 변경하는 암묵적 할당이 이뤄짐
x++; // x = x + 1;
x--; // x = x - 1;
```

피연산자 앞에 위치한 (++/--)는 피연산자의 값을 증가/감소 시킨 후, 다른 연산을 수행

피연산자 뒤에 위치한 (++/--)는다른 연산을 수행 후,  피연산자의 값을 증가/감소



```javascript
// 숫자 타입이 아닌 피연산자에 + 단항 연산자를 사용하면 피 연산자를 숫자 타입으로 변환하여 반환
var x = '1';

// 문자열을 숫자로 타입 변환
connsole.log(+x); // 1

// 불리언 값을 숫자로 타입 변환
x=true;
console.log(+x); // 1

// 불리언 값을 숫자로 타입 변환
x=false;
console.log(+x); // 0

//문자열을 숫자로 타입 변환할 수 없음 -> NaN을 반환
x='Hello';
console.log(+x); // NaN
```



```javascript
// -단한 연산자는 피연산자의 부호를 반전한 값을 반환.

console.log(-(-10)); // 10
console.log(-'10'); // -10
console.log(-true); //-1
console.log(-'Hello'); //NaN
```



<br>

#### 3. 문자열 연결 연산자

연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작

```javascript
//true는 1로 타입 변환
1 + true // 2

//false는 0으로 타입 변환
1 + false // 1

// null은 0으로 타입 변환
1 + null // 1

// undefined는 숫자로 타입 변환되지 않는다.
+undefined // NaN
1 + undefined // NaN
```

<br>



## 2. 할당 연산자

> **할당 연산자** - 우항에 있는 피연산자의 평가 결과를 좌항에 있는 변수에 할당.

<table>
    <th >할당 연산자</th>
     <th>예</th>
    <th >동일 표현</th>
     <th >부수 효과</th>
    <tr>
        <td>=</td>
        <td>x = 5</td>
        <td>x = 5</td>
        <td>O</td>
    </tr>
    <tr>
        <td>+=</td>
        <td>x += 5</td>
        <td>x = x + 5</td>
        <td>O</td>
    </tr>
    <tr>
        <td>-=</td>
        <td>x -= 5</td>
        <td>x = x - 5</td>
        <td>O</td>
    </tr>
    <tr>
        <td>*=</td>
        <td>x *= 5</td>
        <td>x = x * 5</td>
        <td>O</td>
    </tr>
    <tr>
        <td>/=</td>
        <td>x /= 5</td>
        <td>x = x / 5</td>
        <td>O</td>
    </tr>
    <tr>
        <td>%=</td>
        <td>x %= 5</td>
        <td>x = x % 5</td>
        <td>O</td>
    </tr>
</table>

할당문은 값으로 평가되는 표현식인 문으로서 할당된 값으로 평가 된다.

```javascript
var x;

// 할당문은 표현식인 문
console.log(x = 10); // 10
```



<br>



## 3. 비교 연산자



> **비교 연산자**(comparison operator) - 좌항과 우항의 피연산자를 비교한 후 결과를 불리언 값으로 반환.



#### 동등/일치 비교 연산자

동등 비교(loose equality) 연산자와 일치 비교(stric equality) 연산자는 좌항과 우항의 피연산자가 값은 값으로 평가되는지 비교해 불리언 값을 반환.

---

->동등 비교 연산자는 느슨한 비교 / 일치 비교 연산자는 엄격한 비교

---

<table>
    <th>비교 연산자</th>
    <th>의미</th>
    <th>사례</th>
    <th>설명</th>
    <th>부수효과</th>
    <tr>
    	<td>==</td>
        <td>동등 비교</td>
        <td>x == y</td>
        <td>x와 y의 값이 같음</td>
        <td>X</td>
    </tr>
    <tr>
    	<td>===</td>
        <td>일치 비교</td>
        <td>x === y</td>
        <td>x와 y의 값과 타입이 같음</td>
        <td>X</td>
    </tr>
    <tr>
    	<td>!=</td>
        <td>부동등 비교</td>
        <td>x != y</td>
        <td>x와 y의 값이 다름</td>
        <td>X</td>
    </tr>
    <tr>
    	<td>!==</td>
        <td>불일치 비교</td>
        <td>x !== y</td>
        <td>x와 y의 값과 타입이 다름</td>
        <td>X</td>
    </tr>
</table>

동등 비교(==) 연산자는 좌항과 우항의 피연산자를 비교할 때 먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 값은 값인지 비교한다.

```javascript
5 == 5; // true
5 == '5' // true

//예측하기 어려운 결과를 만들어 내기 때문에 일치 비교 연산자를 사용하면 좋다.
'0' == ''; // false
0 == ''; //true
0 == '0'; // true
false == 'false'; // false
false == '0'; // true
false == null; // false
false == undefined // false
```

일치 비교(===) 연산자는 좌항과 우항의 피연산자의 값과 타입이 같은 경우에 true를 반환

```javascript
5 === '5' // false
5 === 5 // true
```

<br>

## 4. 삼항 조건 연산자

삼항 조건 연산자는 조건식의 평가 결과에 따라 반환할 값을 결정

> 조건식 ? 조건식이 true 일때 반환할 값 : 조건식이 false 일 때 반환할 값

```javascript
var result : score >= 60 ? 'pass' : 'fail';
//score >= 60 값이 true 이면 pass
//score >= 60 값이 false 이면 fail

//if...else문을 사용해 유사하게 처리 가능
if(score >= 60) result = 'pass';
else result = 'fail';
```

삼항 연산자 표현식은 값처럼 사용 가능하지만 if...else 문은 값처럼 사용 할 수 없다.

```javascript
var x = 10;

//if...else 문은 표현식이 아닌 문. -> 값처럼 사용 X
var result = if(x % 2) { result = '홀수';} else {result = '짝수'};

result = x % 2 ? '홀수' : '짝수';
```

---

**삼항 조건 연산자 표현식은 값으로 평가할 수 있는 표현식인 문**

---

<br>

## 5. 논리 연산자

> **논리 연산자**(logical operator) - 우항과 좌항의 피연산자

<table>
    <th>논리 연산자</th>
    <th>의미</th>
    <th>부수 효과</th>
    <tr>
    	<td>||</td>
        <td>논리합(OR)</td>
        <td>X</td>
    </tr>
    <tr>
    	<td>&&</td>
        <td>논리곱(AND)</td>
        <td>X</td>
    </tr>
    <tr>
    	<td>!</td>
        <td>부정(NOT)</td>
        <td>X</td>
    </tr>
</table>

```javascript
// 논리합(||) 연산자
true || true; // true
true || false; // true
false || false; // true
false || false; // false

// 논리곱(&&) 연산자
true && true; // true
true && false; // false
false && true; // false
false && false; // false

// 논리 부정(!) 연산자
!true; // false
!false; // true
```



> 드 모르간의 법칙
>
> ```javascript
> !(x || y) === (!x && !y)
> !(x && y) === (!x || !y)
> ```
>
> https://ko.wikipedia.org/wiki/%EB%93%9C_%EB%AA%A8%EB%A5%B4%EA%B0%84%EC%9D%98_%EB%B2%95%EC%B9%99

<br>

## 6. 쉼표 연산자

쉼표(,) 연산자 - 왼쪽 피연산자부터 차례대로 피연산자를 평가하고 마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가 결과를 반환

```javascript
var x, y, z;

x = 1, y = 2, z = 3; //3
```



<br>

## 7. typeof 연산자

> typeof 연산자 - 피연산자의 데이터 타입을 문자열로 반환
>
> "string", "number", "boolean", "undefined", "symbol", "object", "function" 중 하나를 반환

---

typeof 연산자가 반환하는 문자열은 7 개의 데이터 타입과 정확히 일치하지는 않는다

ex) typeof 연산자로 null 값을 연산하면 "object"를 반환

---

선언하지 않는 식별자를  typeof 연산자로 연산하면 ReferenceError가 아닌 undefined를 반환

---



<br>

## 8. 지수 연산자

> 지수 연산자 - 좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭 제곱하여 숫자 값을 반환

```javascript
2 ** 2; // 4
2 ** 0 ; //1

//음수를 거듭제곱의 밑으로 사용해하려면 괄호로 묶어야 함
(-5) ** 2 // 25

//지수 연산자는 할당 연산자와 함께 사용 가능
var num = 5;
num **= 2; // 25

// 지수 연산자는 이항 연산자 중에 우선순위가 가장 높음
2 * 5 ** 2; // 50
```



## 