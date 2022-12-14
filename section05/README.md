# 자바스크립트의 작동방식과 버전 차이점

## ES란

- ES란 ecmascript를 의미하며 브라우저에 자바스크립트 문법의 표준 지침을 제공한다.
- 이를 통해 브라우저는 최신 자바스크립트의 기능을 자동업데이트 해준다.
- 현재 주로 쓰이는 버전은 ES6 이다.

## 변수의 종류와 차이점

### var

- 변수를 만들때 사용한다.
- ES5 까지 주로 쓰이던 변수이다.
- 함수 또는 전역스코프이다.
- 동일한 이름의 변수를 재선언 가능하다.

### let

- 변수를 만들때 사용한다.
- ES6에 새로 도입된 변수이다.
- 블럭 스코프이다.
- 동일한 이름의 변수를 재선언 불가하다.

### const

- 상수를 만들때 사용한다.
- ES6에 새로 도입된 상수이다.
- 블럭 스코프이다.
- 동일한 이름의 변수를 재선언 불가하다.

### var과 let & const차이점

```javascript
for (var i = 0; i < 2; i++) {}
console.log(i); //정상출력
for (let j = 0; j < 2; j++) {}
console.log(j); //에러발생
```

- var은 함수외에서 선언되면 전역스코프이다.
- let과 const는 **블럭 스코프**라는 개념을 사용해서 if문이나 for문 같은 중괄호가 있는 블럭에서 변수가 선언되면 해당 블럭 내에서만 쓰일 수 있다.
- 키워드를 사용하지않고 변수를 선언하면 브라우저 내에서 자동으로 var을 추가한다.

## 호이스팅(Hoisting)

```javascript
// 에러가 발생하지 않고 undefined가 출력된다.
console.log(userName);
var userName = 'jun';

// userName을 초기화하기전에 참조할 수 없다고 에러가 뜬다.
console.log(userName);
let userName = 'jun';
```

- 자바스크립트에는 **호이스팅**이라는 기능이 있어서 스크립트를 로드할때 전체 스크립트를 확인해서 함수를 찾은 뒤 자동으로 등록해서 다른 언어와 달리 함수선언을 밑에서 해도 호출을 코드 위에서 하는 것이 가능하다.
- 변수도 함수와 마찬가지로 로드될때 파일 맨위로 가져오며 이 동작은 브라우저내에서 보이지 않게 동작한다.
- var변수는 밑의 초기화는 그대로 둬서 디폴트인 undefined로 초기화 되고 에러가 발생하지 않게 된다.
- const와 let도 파일맨위로 가져오는 것은 마찬가지지만 var과 달리 디폴트인 undefined로 초기화 하지 않아서 에러가 발생한다.

## 엄격모드

```javascript
'use strict';
userName = 'jun';
console.log(userName); //엄격모드를 사용해 오류가 발생
```

- 파일 내에 **'use strict'** 라는 문자열을 추가해서 엄격모드를 사용할 수 있다.
- 엄격모드를 사용하면 몇몇 기능을 비활성화하여 키워드를 사용하지않고 변수를 선언하는 등 좋지않은 행위를 막을 수 있다.
- 이상한 코드를 사용하지 않는다면 엄격모드를 쓸 일은 없다..

## 자바스크립트 엔진의 작동 방식

1. 브라우저에서는 자바스크립트 엔진을 이용해서 코드의 분석(parsing)과 실행(execution)이 이루어진다.
2. 자바스크립트 엔진은 내장된 인터프리터와 컴파일러 두 부분으로 나뉘어 코드 분석과 실행을 한다.
3. 인터프리터는 코드를 분석하고 실행하기 좋은 바이트코드(bytecode)로 변경한 후에 스크립트 실행을 한다.
4. 인터프리터는 이 바이트코드를 스크립트를 실행할 때에 한줄씩 실행한다.
5. 바이트코드(Bytecode)는 고급 언어로 작성된 소스 코드를 가상머신이 이해할 수 있는 중간 코드로 컴파일한 것을 말한다.
6. 또 바이트코드로 변경된 소스 코드는 컴파일러로 전달된다.
7. 컴파일러는 이 바이트코드를 더욱 빨리 실행이 가능한 기계어(machine code)로 변경한다.
8. 자바스크립트 엔진은 이전 실행과 현재 실행시 코드의 달라진 점이 없다면 재컴파일링을 거치지 않고 컴파일된 코드를 재사용한다.
9. 브라우저는 API와 같이 자바스크립트 코드에서 사용할 수 있는 빌트인 기능도 지원한다.

## 자바스크립트 코드의 실행 원리

```javascript
//정의한 함수는 호이스팅 기능으로 코드가 실행되기전에 자동등록된다.
function getName() {
  return prompt('이름을 입력하세요.', '');
}
function greet() {
  const userName = getName();
  console.log('hello ' + userName);
}
greet(); //바로 greet 줄이 실행된다.

/*
| call stack |
| :--------: |
|  prompt()  |
| getName()  |
|  greet()   |
|(anonymous) |
*/
```

1. 자바스크립트 엔진에서는 메모리와 실행 단계에 관한 관리가 이루어진다.
2. 메모리와 실행 단계에 관한 관리는 **힙**(heap)과 **스택**(stack)으로 관리된다.
3. **힙**은 장기 메모리(long-term memory, LTM)를 의미하며 시스템 메모리 데이터를 저장하고 메모리 할당 작업을 수행한다.
4. **스택**은 단기 메모리(short-term memory)와 관련된 작업을 수행하며 주로 현재 실행중인 함수를 관리하는 역할을 한다.
5. anonymous함수는 **main함수**이며 제일 먼저 호출된다.
6. 정의된 함수를 자바스크립트 엔진에서 이를 등록하면 힙에 저장된다.
7. greet함수가 실행될때는 스택으로 이동하게 되고 필요치 않을때는 방출된다.
8. 스택에서는 맨 위에 있는 항목이 항상 현재 실행중인 항목이 된다.
9. 스택에서 스크립트의 흐름을 볼수 있다는 의미가 자바스크립트는 단일스레드이며 한번에 하나의 작업만을 수행한다는 의미이다. 이는 함수의 실행 순서를 보장하고 모든 함수가 어떤 함수와 관련되었는지를 알수있게 해준다.
10. 브라우저에는 힙과 스택외에 이벤트리스너를 관리하는 이벤트루프도 있다. 추후에 학습예정..

## 데이터의 기본형과 참조형

### 기본형 데이터 타입

- String, Number, Boolean, null, undefined, symbol
- 일반적으로 스택이라는 메모리 공간에 저장된다.
- 변수는 값을 저장하며 복사한다는 것은 새로운 변수에 할당하는 것을 의미한다.

### 참조형 데이터 타입

```javascript
let hobbies = ['sports'];
let newHobbies = hobbies;
hobbies.push('cooking');
console.log(newHobbies); // ['sports', 'cooking']
```

- array, object
- 힙이라는 메모리 공간에 저장된다.
- 변수는 값이 아닌 메모리 주소의 위치를 저장한다.
- 변수를 복사한다는 것은 새로운 변수에 같은 주소를 할당하는 것을 의미한다.

## 가비지 컬렉션 & 메모리 관리

- 모든 자바스크립트 엔진에는 가비지컬렉터(garbage collector)를 가지고 있다.

### 가비지컬렉터의 역할

- 사용(참조)되지 않은 객체에 대한 힙 메모리를 주기적으로 체크하여 메모리에서 제거한다.

```javascript
let person = { name: 'Max' }; // 해당 객체의 주소를 참조하는 변수가 없기때문에 메모리에서 제거됨
person = null; // 새로운 값이 할당
```

### 메모리 누수란

- 더이상 사용되지 않는 객체임에도 불구하고 해당 객체가 참조되고 있는 경우에는 가비지 컬렉터로 메모리에서 제거가 되지않는다.

```javascript
// case1
const addListenerBtn = document.getElementById('add-listener-btn');
const clickableBtn = document.getElementById('clickable-btn');

function printMessage() {
  const value = messageInput.value;
  console.log(value || 'Clicked me!');
}

function addListener() {
  clickableBtn.addEventListener('click', printMessage);
}

addListenerBtn.addEventListener('click', addListener);
```

- addListenerBtn을 계속 클릭하여 addListener를 여러번 호출한다해서 printMessage함수를 여러번 호출할수 없다.
- 브라우저는 이전에 사용한 함수를 재호출 하는 경우, 새로운 리스너를 만드는 대신에 기존 리스너를 새로운 리스너로 교체한다.

```javascript
// case2 메모리누수 예시
function addListener() {
  clickableBtn.addEventListener('click', function () {
    const value = messageInput.value;
    console.log(value || 'Clicked me!');
  });
}
//
addListenerBtn.addEventListener('click', addListener);
```

- 위의 예시는 익명함수로 addListener를 호출할때마다 계속 새로운 함수가 생성되고 이는 메모리에 쌓이게되며 이는 가비지컬렉터가 제거할수없어 메모리누수를 야기한다.
- 매우 드문 경우지만 주의해야한다.
