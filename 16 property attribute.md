# 프로퍼티 어트리뷰트



## 1. 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 

ECAMScript 사양에서 사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)다.

ECMAScript 사양에 등장하는 이중 대괄호([[...]])로 감싼 이름들이 내부 슬롯과 내부 메서드다.

> https://262.ecma-international.org/11.0/#sec-object-type

자바스크립트는 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않지만

일부에 한하여 간접적으로 접근할 수 있는 수단을 제공한다.



<br>

## 2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

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

## 3. 데이터 프로퍼티와 접근자 프로퍼티

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
            <li>생략시 기본값 undefined</li>
        </ul></td>
    </tr>
    <tr>
        <td>[[Writable]]</td>
        <td>writable</td>
        <td><ul>
        	<li>프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값</li>
       		 <li>[[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 됨</li>
            <li>생략시 기본값 false</li>
        </ul></td>
    </tr>
    <tr>
        <td>[[Enumerable]]</td>
        <td>enumerable</td>
        <td><ul>
        	<li>프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다</li>
       		 <li>[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for...in 문이나 Object.keys 메서드 등으로 열거 불가</li>
            <li>생략시 기본값 false</li>
        </ul></td>
    </tr>
    <tr>
        <td>[[Configurable]]</td>
        <td>configurable</td>
        <td><ul>
        	<li>프로퍼티 재정의 가능 여부를 나타내며 불리언 값</li>
       		 <li>[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지. 단[[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용</li>
            <li>생략시 기본값 false</li>
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
        <td>생략시 기본값 undefined</td>
    </tr>
    <tr>
    	<td>[[Set]]</td>
        <td>set</td>
        <td>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수. 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.</td>
        <td>생략시 기본값 undefined</td>
    </tr>
    <tr>
    	<td>[[Enumerable]]</td>
        <td>enumerable</td>
        <td>데이터 프로퍼티의 [[Enumerable]]과 같다</td>
        <td>생략시 기본값 false</td>
    </tr>
    <tr>
    	<td>[[Configurable]]</td>
        <td>configurable</td>
        <td>데이터 프로퍼티의 [[Configurable]]과 같다.</td>
        <td>생략시 기본값 false</td>
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

<Br>

## 4. 프로퍼티 정의

> 프로퍼티 정의 - 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 
>
> 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의 하는 것
>
> Object.defineProperty 메서드를 사용하며 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 
>
> 프로퍼티 디스크립터 객체를 전달

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName',{
    value : 'gildong',
    writable : true,
    enumerable : true,
    configurable : true
});

Object.defineProperty(person, 'lastName',{
    value: 'Hong'
});

Object.getOwnPropertyDescriptor(person, 'firstName') 
// {value: 'gildong', writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값
Object.getOwnPropertyDescriptor(person, 'lastName') 
// {value: 'Hong', writable: false, enumerable: false, configurable: false}

//[[Enumerable]]이 false인 경우 for...in 문이나 Object.keys 등으로 열거 X
Object.keys(person) //['firstName'] ->lastName은  enumerable: false이기 때문

// [[Writable]]이 false인 경우 [[Value]]의 값 변경 X
person.lastName = 'Kim'; // 에러 발생하지 않고 무시

// [[Configurable]]이 false인 경우 해당 프로퍼티 재정의 X
Object.defineProperty(person, 'lastName', {enumerable : true});
// TypeError: Cannot redefine property: lastName


// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName',{
    //getter 함수
    get(){
        return `${this.firstName} ${this.lastName}`
    },
    
    // setter 함수
    set(name){
        [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable : true,
    configurable : true
})

Object.getOwnPropertyDescriptor(person, 'fullName') 
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}

person.fullName = 'jeongjae Lee';

person // {firstName: 'jeongjae', lastName: 'Hong'}
```

```js
// Object.defineProperties 이용시 여러개의 프로퍼티 정의 가능
const person = {};

Object.defineProperties(person, {
    firstName :{
    value : 'gildong',
    writable : true,
    enumerable : true,
    configurable : true
	},
    laststName: {
    value : 'Hong',
    writable : true,
    enumerable : true,
    configurable : true
	},
    fullName : {
    //getter 함수
    get(){
        return `${this.firstName} ${this.lastName}`
    },
    
    // setter 함수
    set(name){
        [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable : true,
    configurable : true
	}
})
```

<br>

## 5. 객체 변경 방지

객체는 프로퍼티를 추가하거나 삭제할 수 있고, 프로퍼티 어트리 뷰트를 재정의 할 수 있다.

<table>
    <th>구분</th>
    <th>메서드</th>
    <th>프로퍼티 추가</th>
    <th>프로퍼티 삭제</th>
    <th>프로퍼티 값 읽기</th>
    <th>프로퍼티 값 쓰기</th>
    <th>프로퍼티 어트리뷰트 재정의</th>
    <tr>
    	<td>객체 확장 금지</td>
        <td>Object.preventExtensions</td>
        <td>X</td>
        <td>O</td>
        <td>O</td>
        <td>O</td>
        <td>O</td>
    </tr>
     <tr>
    	<td>객체 밀봉</td>
        <td>Object.seal</td>
        <td>X</td>
        <td>X</td>
        <td>O</td>
        <td>O</td>
        <td>X</td>
    </tr>
     <tr>
    	<td>객체 동결</td>
        <td>Object.freeze</td>
        <td>X</td>
        <td>X</td>
        <td>O</td>
        <td>X</td>
        <td>X</td>
    </tr>
</table>



### 객체 확장 금지

**객체 확장 금지**란 프로퍼티 추가 금지를 의미

Object.preventExtensions 메서드는 객체 확장을 금지한다. 

-> 프로퍼티 추가가 금지. 즉 프로퍼티 동적 추가와 Object.defineProperty 메서드 방법 금지

확장이 가능한 객체의 여부는 Object.isExtensible 메서드로 확인할 수 있다.

```js
const person = {
    name : 'gildong'
};
Object.isExtensible(person); // true

Object.preventExtensions(person);

Object.isExtensible(person); // false
person.age = 18; // 무시됨
person // {name: 'gildong'}

delete person.name // 삭제는 된다
person // {}
```



### 객체 밀봉

**객체 밀봉** 이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지이며

**Object.seal** 메서드 사용

-> 밀봉된 객체는 읽기와 쓰기만 가능

밀봉된 객체의 여부는 Object.isSealed 메서드로 확인 가능

```js
const person = {
    name : 'gildong'
};
Object.isSealed(person); // false

Object.seal(person);

Object.isSealed(person); // true

Object.getOwnPropertyDescriptors(person)
//name:{configurable: false, enumerable: true, value: "gildong", writable: true}

// 프로퍼티 추가, 삭제, 프로퍼티 어트리뷰트 재정의 금지지만 값 갱신은 가능
person.age = 20;
delete person.name;
Object.defineProperty(person, 'name', {configurable:true}) //TypeError: Cannot redefine property: name
person.name = 'kim'
person // {name: 'kim'}
```



### 객체 동결

**객체 동결**이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 

프로퍼티 값 갱신 금지를 의미하며 **Object.freeze** 메서드 사용

즉 읽기만 가능하다.

동결된 객체의 여부는 Object.isFrozen 메서드로 확인가능



```js
const person = {name : 'Lee'};

Object.isFrozen(person) // false

Object.freeze(person);

Object.isFrozen(person) // true

// 프로퍼티 추가, 삭제, 프로퍼티 어트리뷰트 재정의, 값 갱신 금지
person.age = 20;
delete person.name;
Object.defineProperty(person, 'name', {configurable:true}) //TypeError: Cannot redefine property: name
person.name = 'kim'
person // {name: 'Lee'}
```



### 불변 객체

Object.freeze 메서드로 객체를 동결하여도 직속 프로퍼티만 동결하고 중첩 객체까지 동결할 수 없으므로 

읽기기 전용의 불변 객체를 구현하려면 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

```js
function deepFreeze(target){
    // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결
    if (target && typeof target === 'object' && !Object.isFrozen(target)){
        Object.freeze(target);
        // 모든 프로퍼티를 순회하며 재귀적으로 동결
        Object.keys(target).forEach(key => deepFreeze(target[key]));
    }
    return target;
}
```

