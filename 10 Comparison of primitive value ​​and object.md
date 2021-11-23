# 원시 값과 객체의 비교

> - 원시 타입의 값은 변경 불가능한 값 / 객체 타입의 값은 변경 가능한 값
>
> - 원시 값을 변수에 할당하면 변수(메모리 공간)에는 실제 값이 저장 / 객체를 변수에 할당하면 변수에는 참조 값이 저장
>
> - 원시 값 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달 -> 다른 메모리 주소를 사용
>
>   / 객체 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달 -> 변수가 저장된 메모리 공간은 다르지만 같은 참조 값. 즉 객체 값을 공유함.



## 원시 값

#### 변경 불가능한 값

원시 타입 값은 재할당하면 메모리 공간에 저장되어 있는 재할당 이전의 원시 값을 변경하는게 아니라!  

**새로운 메모리 공간을 확보**하고 **재할당한 원시 값을 저장**한 후, 변수는 새롭게 **재할당한 원시 값**을 가리킨다.

-> 메모리 공간의 주소가 동일한게 아니라 **재할당시 메모리 주소가 변경**된다.(불변성)

> 상수(const) - 재할당이 금지된 변수
>
> -> 상수는 재할당이 금지된 변수일 뿐 변경 불가능한 값이랑 똑같이 생각하면 안된다.

원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법은 없다.



#### 문자열과 불변성

> 문자열은 0개 이상의 문자로 이뤄진 집합, 1개의 문자는 2바이트의 메모리 공간에 저장됨

문자열은 유사 배열 객체이면서 이터러블이라 배열과 유사하게 각 문자에 접근 가능하다.

> **이터러블**(Iterable) - Symbol.iterator 메소드를 가지고 있는 객체
>
> 쉽게 생각하면 반복 가능한 타입

> **유사 배열 객체**(array-like object) - 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체
>
> ```js
> var str = 'string';
> str[0]; // 's'
> str.length // 6
> ```
>
> -> 원시 값을 객체처럼 사용하면 원시 값을 감싸는 래퍼 객체로 자동 변환'



```js
var str = 'String';

//문자열은 변경 불가능한 값이기 때문에 불변한다. 재할당은 가능!
str[0] = 'S'
str // 'string'
```



#### 값에 의한 전달

> 값에 의한 절달 - 변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수(복사값)에는 할당되는 변수(원본)의 원시 값이 복사되어 전달
>
> -> 다른 메모리 공간에 저장되는 별개의 값!!!

```js
var score = 80;

var copy = score;

score === copy; //true -> 데이터 타입과 값은 동일하기 때문

score = 100; // -> copy 변수의 값에는 영향을 주지 않음

score === copy; //false -> 값이 틀리다
```



-------------------------

변수에는 값이 전달되는 것이 아니라 **메모리 주소**가 전달 

-> 변수와 같은 식별자는 값이 아니라 메모리 주소를 기억하고 있기 때문

--------------------------------------------

<br>



## 객체

객체는 **변경 가능한 값**(mutable value) 즉 프로퍼티 개수가 정해져 있지 않으며, 동적으로 추가, 삭제할 수 있다.

-> 객체는 원시 값과 같이 확보해야 할 메모리 공간의 크기를 사전에 정할 수 없다.



#### 변경 가능한 값

객체를 할당한 변수는 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 **참조 값reference value**(생성된 메모리 공간의 주소가 저장되어 있음)을 통해 실제 객체에 접근한다.

<img src=./image/comparison1.png height=300>

객체를 변경(추가,갱신 삭제 등) 하더라도 객체를 할당한 변수의 참조 값은 변경되지 않고 객체를 직접 변경한다



#### 참조에 의한 전달

> 참조에 의한 전달 - 객체를 가리키는 변수(원본)을 다른 변수(사본)에 할당하면 원본의 참조 값이 복사되어 전달

```js
var person = {
    name : 'Lee'
};

//참조 값을 복사(얕은 복사)
var copy = person;
```



<img src=./image/comparison2.png height=300>

원본 person과 사본 copy가 **동일한 객체를 공유**하므로 원본 또는 사본 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다.

```js
var person = {
    name : 'Lee'
};

//참조 값을 복사(얕은 복사)
var copy = person;
copy === pserson // true

copy.name = 'Kim';
person.adress = 'Seoul';

copy // {name: 'Kim', adress: 'Seoul'}
person //{name: 'Kim', adress: 'Seoul'}
// ->copy와 person은 동일한 객체를 가리킨다.
```

--------------------

**값에 의한 전달**과 **참조에 의한 전달**은 식별자(변수)가 기억하는 메모리 공간에 저장되어 있는 값을 복사한다는 면에서는 동일하다.

원시 값이냐 참조 값이냐의 차이만 있을 뿐.

-------------

### 얕은 복사와 깊은 복사

\* 책과 블로그마다 얕은 복사에 대해 다른게 있음 주의해서 볼 것

> 얕은 복사 - 같은 주소값을 바라보며, 객체에 중첩되어 있는 객체의 경우 참조 값을 복사

> 깊은 복사 - 객체에 중첩되어 있는 개체까지 모두 복사 -> 원시 값처럼 완전한 복사본을 만듦
>
> -> 값 자체의 완전한 복사

```js
// 얕은 복사
const obj = {
    x : 1,
    y : {
        name : 'hong',
        age : 18
        }
};

const cobj=obj;

obj === cobj // true

obj.x = 3;
obj === cobj // true -> 같은 주소값을 가지고 있기 때문
```



```js
// 깊은 복사
const obj = {
    x : 1,
    y : {
        name : 'hong',
        age : 18
        }
};

const cobj = {...obj}

obj === cobj // false -> 새로운 메모리에 객체를 저장하고 다른 참조 값을 가지고 있다.

obj.y.name = 'HongKa';

cobj.y.name // 'HongKa' -> 전개 연산자를 이용한 복사는 dept2 부터는 객체의 참조 값을 복사하기 때문
```

전개 연산자는 depth1 까지만 완적 복사고 depth 2부터는 얕은 복사이기 때문에 다른 방법을 통해 깊은 복사를 해야 한다.

1. JSON 객체 메소드 이용 - 다른 방법에 비해 성능이 느리고 함수를 만나면 undefined로 처리함

```js
const obj = {
    x : 1,
    y : {
        name : 'hong',
        age : 18
        }
};

const cobj = JSON.parse(JSON.stringify(obj));

obj === cobj // false

obj.x = 2;
cobj.x = 5;

obj.x === cobj.x // false
```

2. lodash 모듈의 cloneDeep() 메소드 이용

```js
const obj = {
    x : 1,
    y : {
        name : 'hong',
        age : 18
        }
};

const cobj = _.cloneDeep(obj);
```



3. 커스텀 재귀 함수

```js
function deepCopy(obj) {
  if (obj === null || typeof obj !== "object") {
    return obj;
  }

  let copy = {};
  for (let key in obj) {
    copy[key] = deepCopy(obj[key]);
  }
  return copy;
}

const obj = {
    x : 1,
    y : {
        name : 'hong',
        age : 18
        }
};

const cobj=deepCopy(obj);

cobj.y.name = 'HongKa';

obj.y.name === cobj.y.name // false
```

