# 프로퍼티 어트리뷰트



## 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 

ECAMScript 사양에서 사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)다.

ECMAScript 사양에 등장하는 이중 대괄호([[...]])로 감싼 이름들이 내부 슬롯과 내부 메서드다.

> https://262.ecma-international.org/11.0/#sec-object-type

자바스크립트는 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않지만

일부에 한하여 간접적으로 접근할 수 있는 수단을 제공한다.



<br>

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

> 프로퍼티 어트리뷰트 - 자바스크립트 엔진이 관리하는 내부 상태 값(meta-property)인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는**프로퍼티 어트리뷰트**를 기본값으로 자동 정의

> 프로퍼티의 상태 - 프로퍼티의 값(value), 값의 갱신 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)

```js
//프로퍼티 어트리뷰트에 직접 접근 할 수 없고 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접 확인 가능

const person = {
    name : 'LEE',
    age : 18
};

//프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환
console.log(Object.getOwnPropertyDescriptor(person, 'name'))  
//{value: 'LEE', writable: true, enumerable: true, configurable: true}

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환
console.log(Object.getOwnPropertyDescriptors(person))  
// {name: {value: 'LEE', writable: true, enumerable: true, configurable: true},
// age: {value: 18, writable: true, enumerable: true, configurable: true}}

```

<br>

## 데이터 프로퍼티와 접근자 프로퍼티

> 데이터 프로퍼티(data property) - 키와 값으로 구성된 일반적인 프로퍼티
>
> 접근자 프로퍼티(accessor property) - 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장 할 때
>
> 호출되는 접근자 함수(accessor function)로 구성된 프로퍼티

### 데이터 프로퍼티

데이터 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 가진다.

이 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

<table>
    <th>프로퍼티 어트리뷰트</th>
    <th>프로퍼티 디스크립터 객체의 프로퍼티</th>
    <th>설명</th>
    <tr>
        <td>[[Value]]</td>
        <td>value</td>
        <td><ul>
        	<li>프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값</li>
        	<li>프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당한다. 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다</li>
        </ul></td>
    </tr>
    <tr>
        <td>[[Writable]]</td>
        <td>writable</td>
        <td><ul>
        	<li>프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값</li>
       		 <li>[[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 됨</li>
        </ul></td>
    </tr>
    <tr>
        <td>[[Enumerable]]</td>
        <td>enumerable</td>
        <td><ul>
        	<li>프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다</li>
       		 <li>[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for...in 문이나 Object.keys 메서드 등으로 열거 불가</li>
        </ul></td>
    </tr>
    <tr>
        <td>[[Configurable]]</td>
        <td>configurable</td>
        <td><ul>
        	<li>프로퍼티 재정의 가능 여부를 나타내며 불리언 값</li>
       		 <li>[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지. 단[[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용</li>
        </ul></td>
    </tr>
</table>





### 접근자 프로퍼티

> 접근자 프로퍼티 - 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 
>
> 접근자 함수(getter/setter 함수)로 구성된 프로퍼티

<table>
    <th>프로퍼티 어트리뷰트</th>
    <th>프로퍼티 디스크립터 객체의 프로퍼티</th>
    <th>설명</th>
    <tr>
        <td>[[Get]]</td>
    	<td>get</td>
        <td>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수. 즉 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환</td>
    </tr>
    <tr>
    	<td>[[Set]]</td>
        <td>set</td>
        <td>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수. 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.</td>
    </tr>
    <tr>
    	<td>[[Enumerable]]</td>
        <td>enumerable</td>
        <td>데이터 프로퍼티의 [[Enumerable]]과 같다</td>
    </tr>
    <tr>
    	<td>[[Configurable]]</td>
        <td>configurable</td>
        <td>데이터 프로퍼티의 [[Configurable]]과 같다.</td>
    </tr>
</table>

```js
const person = {
    //데이터 프로퍼티
    firstName : 'gildong',
    lastName : 'Hong',
    
    // fullName은 접근자 함수로 구성된 접근자 프로퍼티
    // getter 함수
    get fullName(){
        return `${this.firstName} ${this.lastName}`;
    },
    
    // setter 함수
    set fullName(name){
        [this.firstName, this.lastName] = name.split(' ');
    }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + ' ' + person.lastName); //gildong Hong

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출
person.fullName = 'gildong Hong';

person // {firstName: 'gildong', lastName: 'Hong'}
person.fullName // 'gildong Hong'
```

person 객체의 firstName, lastName은 데이터 프로퍼티

메서드 앞에 get, set이 붙은 것이 getter, setter함수이고, getter/setter 함수의 이름 fullName이 접근자 프로퍼티다.

접근자 프로퍼티는 자체적으로 값을 가지지 않으며 데이터 프로퍼티의 값을 읽거나 저장할 때 관여한다.



## 프로퍼티 정의

> 프로퍼티 정의 - 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 
>
> 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의 하는 것
>
> Object.defineProperty 메서드를 사용하며 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 
>
> 프로퍼티 디스크립터 객체를 전달

```js
```

