# 실행 컨텍스트

**실행 컨텍스트** - 식별자(변수,함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부

메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.



<br>

## 1. 소스코드의 타입

소스코드(실행 가능한 코드excutable code)를 4가지 타입으로 구분하는 이유는 소스코드의 타입에 따라 실행 컨텍스트를 생성하는

과정과 관리 내용이 다르기 때문이다.

<table>
    <caption>소스코드의 타입</caption>
    <th>소스코드의 타입</th>
    <th>설명</th>
    <tr>
        <td>전역 코드(global code)</td>
        <td>전역에 존재하는 소스코드. 전역에 정의된 함수, 클래스 등의 내부 코드는 포함 X</td>
    </tr>
    <tr>
        <td>함수 코드(function code)</td>
        <td>함수 내부에 존재하는 소스코드. 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함 X</td>
    </tr>
    <tr>
        <td>eval 코드(eval code)</td>
        <td>빌트인 전역 함수 eval 함수에 인수로 전달되어 실행되는 소스코드</td>
    </tr>
    <tr>
        <td>모듈 코드(module code)</td>
        <td>모듈 내부에 존재하는 소스코드. 모듈 내부의 함수, 클래스 등의 내부 코드는 포함 X</td>
    </tr>
</table>

<br>

1. 전역 코드

전역 코드는 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성해야 한다.

var 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하고

참조하기 위해 전역 객체와 연결되어야 한다.

이를 위해 전역 코드가 평가되면 전역 실행 컨텍스트가 생성된다.

<br>

2. 함수 코드

함수 코드는 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리해야 한다.

생성한 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결해야 한다.

이를 위해 함수 코드가 평가되면 함수 실행 컨텍스트가 생성된다.

<br>

3. eval 코드

eval 코드는 strict mode에서 자신만의 독자적인 스코프는 생성한다.

이를 위해 eval 코드가 평가되면 eval 실행 컨텍스트가 생성된다.

<br>

4. 모듈 코드

모듈 코드는 모듈별로 독자적인 모듈 스코프를 생성한다.

이를 위해 모듈코드가 평가되면 모듈 실행 컨텍스트가 생성된다.



<br>

<br>

<br>



## 2. 소스 코드의 평가와 실행

자바스크립트 엔진은 소스코드를 '소스코드의 평가'와 '소스코드의 실행' 과정으로 나누어 처리한다.

<br>

소스코드 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수

식별자를 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록한다.

<br>

소스코드 평가 과정이 끝나면 선언문을 제외한 소스코드가 순차적으로 실행된다.(런타임이 시작)

이 때 소스코드 실행에 필요한 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득한다.

그리고 변수 값의 변경 등 소스코드의 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.



<br>

<br>

<br>



## 3. 실행 컨텍스트의 역할

**실행 컨텍스트**는 소스 코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.

코드가 실행되려면 아래와 같이 스코프, 식별자, 코드 실행 순서 등의 관리가 필요한데 이 모든 것을 관리하는 것이 **실행 컨텍스트**이다.

1. 선언에 의해 생성된 모든 식별자(변수, 함수, 클래스 등)를 스코프를 구분하여 등록하고 상태 변화(식별자에 바인딩된 값의 변화)를 지속적으로 관리할 수 있어야 한다.
2. 스코프는 중첩 관계에 의해 스코프 체인을 형성해야 한다. 즉 스코프 체인을 통해 상위 스코프로 이동하며 식별자를 검색할 수 있어야 한다.
3. 현재 실행 중인 코드의 실행 순서를 변경 할 수 있어야 하며 다시 되돌아갈 수도 있어야 한다.

 

<br>

<br>

<br>



## 4. 실행 컨텍스트 스택

자바스크립트 엔진은 전역 코드를 먼저 평가하여 전역 실행 컨텍스트를 생성한다. 

그리고 함수가 호출되면 함수 코드를 평가하여 함수 실행 컨텍스트를 생성한다.

이때 생성된 실행 컨텍스트는 스택 자료구조로 관리되는데 이를 **실행 컨텍스트 스택**이라 한다.

<br>

```js
// 1. 전역 코드의 평가와 실행
const x = 1;

// 2. foo 함수 코드의 평가와 실행
function foo(){
    const y = 2;
    
    // 3. bar 함수 코드의 평가와 실행
    function bar(){
        const z = 3;
        console.log(x + y + z);
    }
    // 4. foo 함수 코드로 복귀
    bar();
}
// 5. 전역 코드로 복귀
foo(); // 6
```

위 코드를 실행하면 아래의 그림과 같이 코드가 실행되는 시간의 흐름에 따라 실행 컨텍스트가 추가되고 제거된다.

<img src = './image/excution context1.png'>

<br>

실행 컨텍스트 스택은 코드의 실행 순서를 관리하며 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 

언제나 현재 실행 중인 코드의 실행 컨텍스트다.

따라서 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트를 **실행 중인 실행 컨텍스트**라 부른다.



<br>

<br>

<br>



## 5. 렉시컬 환경

> **렉시컬 환경** - 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로
>
> 실행 컨텍스트를 구성하는 컴포넌트.

실행 컨텍스트 스택이 코드의 실행 순서를 관리한다면 렉시컬 환경은 스코프와 식별자를 관리한다.

<br>

렉시컬 환경은 키와 값을 갖는 객체 형태의 스코프(전역, 함수, 블록 스코프)를 생성하여 식별자를 키로 등록하고 식별자에

바인딩된 값을 관리한다.

즉 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는



<br>

<br>

<br>



## 6. 실행 컨텍스트의 생성과 식별자 검색 과정



```js
var x = 1;
const y = 2;

function foo(a){
    var x = 3;
    const y = 4;
    
    function bar(b){
        const z = 5;
        console.log(a + b+ x + y + z);
    }
    bar(10);
}

foo(20);
```



<br>

<br>

### 6.1 전역 객체 생성

전역 객체는 전역 코드가 평가되기 이전에 생성된다.

전역 객체에는 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체가 추가되며 동작환경에 따라 클라이언트 사이드

Web API 또는 특정 환경을 위한 호스트 객체를 포함한다.



<br>

<br>

### 6.2 전역 코드 평가

소스코드가 로드되면 자바스크립트 엔진은 전역 코드를 평가한다.

1. 전역 실행 컨텍스트 생성

2. 전역 렉시컬 환경 생성

   2.1 전역 환경 레코드 생성

   ​	2.1.1 객체 환경 레코드 생성

   ​	2.1.2 선언한 환경 레코드 생성

   2.2 this 바인딩

   2.3 외부 렉시컬 환경에 대한 참조 결정





<br>

<br>

### 6.3 전역 코드 실행















<br>

<br>

### 6.4 foo 함수 코드 평가













<br>

<br>

### 6.5 foo 함수 코드 실행















<br>

<br>

### 6.6 bar 함수 코드 평가







<br>

<br>

### 6.7 bar 함수 코드 실행















<br>

<br>

### 6.8 bar 함수 코드 실행 종료













<br>

<br>

### 6.9 foo 함수 코드 실행 종료













<br>

<br>

### 6.10 전역 코드 실행 종료











<br>

<br>

<br>



## 실행 컨텍스트와 블록 레벨 스코프