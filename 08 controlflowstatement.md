# 제어문

> 제어문(control flow statement) - 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문) 할 때 사용.
>
> 제어문을 사용하여 코드의 실행 흐름을 인위적으로 제어



<br>

## 1. 블록문

> **블록문**(block statement/compound statement) - 0개 이상의 문을 중괄호로 묶은 것.
>
> 코드 블록 또는 블록이라고 부르기도 함.



```javascript
// 블록문
{
    var foo = 10;
}

// 제어문
var x = 1;
if(x < 10) {
    x++;
}

// 함수 선언문
function start(a,b){
    return a+b;
}
```

<br>



## 2. 조건문

> **조건문**(conditional statement) - 주어진 조건식(conditional expression)의 평가 결과에 따라 코드 블록(블록문)의 실행을 결정. 조건식은 불리언 값으로 평가될 수 있는 표현식.
>
> 자바스크립트는 **if...else문**과 **switch 문**을 제공



#### if...else 문

if...else 문은 조건식의 평가 결과(논리적 참 또는 거짓)에 따라 실행할 코드 블록을 결정

```js
if(조건식 1){
    //조건식 1이 참이면 이 코드 블록 실행
} else if(조건식2){
    //조건식 2이 참이면 이 코드 블록 실행
} else {
    //조건식 1,2가 모두 거짓이면 이 코드 블록 실행
} 
```

```js
//코드 블록 내의 문이 하나라면 중괄호 생략 가능
var num = 10;
var kind;

if(num % 2) kind = '홀수';
else kind = '짝수';

// 삼항 조건 연산자로도 바꿔 쓸 수 있음
kind = x % 2 ?  '홀수' : '짝수';
```



#### switch 문

> **switch** 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름으로 옮긴다.
>
> case 문은 상황을 의미하는 표현식을 지정하고 콜론으로 마치고 그 뒤에 실행할 문들을 위치 시킨다.

if...else 문은 논리적 참, 거짓으로 실행할 코드 블록을 결정할 때 사용

switch 문은 논리적 참, 거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용

```js
switch(표현식){
    case 표현식1 :
        switch 문의 표현식과 표현식 1이 일치하면 실행될 문;
        break;
    case 표현식2 :
    	switch 문의 표현식과 표현식2가 일치하면 실행될 문;
        break;
    default :
        switch 문의 표현식과 case 문이 없을 때 실행될 문;
}
//break 문이 없다면 case문의 표현식과 일치하지 않더라도 다음 case 문으로 이동하므로 break 문이 필요한 경우는 적절히 사용하는게 좋다.(쓰지 않아도 되는 경우도 있음) 
//-> break 문을 사용하지 않아 switch 사용하지 않고 연이어 case 문과 default 문을 실행하는 경우(폴스루)
```



<br>



## 3. 반복문

> 반복문(loop statement) - 조건식의 평가 결과가 참인 경우 코드 블록을 실행.
>
> 그 후 조건식을 다시 평가하여 거짓일 때 까지 반복.
>
> **for 문, while 문, do...while 문**



#### for 문

for 문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행.

```js
for ( 변수 선언문 또는 할당문 ; 조건식 ; 증감식){
    조건식이 참인 경우 반복 실행될 문;
}
```



#### while 문

while 문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행(반복 횟수가 불명확할 때 주로 사용)

```js
while(조건식){
    조건식이 참인 경우 실행될 문;
}
```



#### do...while 문

do...while문은 코드 블록을 먼저 실행하고 조건식을 평가.

-> 코드 블록은 무조건 한 번 이상 실행됨.

```js
do{
    실행 될 문;
}while(조건식);
```

<br>

## 4. break 문

break문은 레이블 문, 반복문(for, for...in, for...of, while, do...while), switch 문의 코드 블록을 탈출.

그 이외의 문에 break 문을 사용하면 SyntaxError(문법 에러)가 발생.

> 레이블 문(label statement) - 식별자가 붙은 문
>
> ```js
> // apple이라는 레이블 식별자가 붙은 레이블 문
> apple: console.log('apple');
> 
> banana : {
>     console.log('fruit');
>     break banana;
>     console.log('apple');
> }
> ```
>
> 레이블 문을 사용하면 프로그램의 흐름이 복잡해져 가독성이 나빠지고 오류 발생 가능성이 높기 때문에 권장하지 않음.



<br>

## 5. continue 문

continue 문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킴(탈출 X)

```js
var word = 'React Native';
var search = 'a';
var count = 0;

for(var i = 0 ; i<word.length; i++){
    if(word[i] === search){
        count++;
    }
}

// if 문 내의 실행해야 할 코드가 길다면 들여쓰기가 한 단계 깊어지므로 continue 문을 사용하는게 가독성이 좋다.
for(var i = 0 ; i<word.length; i++){
    if(word[i] !== search) continue;
    count++;
}
```

