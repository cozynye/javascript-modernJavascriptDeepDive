# 빌트인 객체

## 1. 자바스크립트 객체의 분류

1. **표준 빌트인 객체** - ECMAScript 사양에 정의된 객체, 애플리케잉션 전역의 공통 기능을 제공
2. **호스트 객체** - ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행환경에서 추가로 제공하는 객체
3. **사용자 정의 객체** - 사용자가 직접 정의한 객체



<br>

<br>

<br>

## 2. 표준 빌트인 객체

자바스크립트는 Object, String, Number, Boolean, Symbol, Math, Reflect, JSON, Proxy 등 40여 개의 표준 빌트인 객체를 제공한다.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.

생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공,

생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공

<br>

생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다
Object.getPrototypeOf(strObj) === String.prototype // true
```

