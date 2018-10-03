# 10/3 (수) TIL

------



# 1. Today I Learned

## Review (Weak Point Study)



## JavaScript 기초 

## 함수 (Function)



### 값으로서의 함수

JavaScript에서는 함수도 값입니다!

```js
function add(x, y) {
  return x + y;
}

const plus = add;
plus(1, 2); // 3
```

다른 값과 마찬가지로, 함수를 선언한 뒤 변수에 대입해서 호출할 수도 있고, 혹은 배열이나 객체에 넣을 수도 있고, 심지어는 함수를 다른 함수에 인수로 넘기거나, 함수에서 함수를 반환할 수도 있습니다.

```js
// 함수를 배열이나 객체에 넣기
function add(x, y) {
  return x + y;
}
[add];
{addFunc: add};

// 함수를 인수로 넘기기
function isEven(x) {
  return x % 2 === 0;
}
[1, 2, 3, 4, 5].filter(isEven); // [2, 4]

// 함수에서 함수 반환하기
function createEmptyFunc() {
  function func() {}
  return func;
}
```

컴퓨터 과학 분야에서 사용되는 용어 중에 [1급 시민(First-Class Citizen)](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)이라는 특이한 용어가 있습니다. 값으로 사용할 수 있는 JavaScript의 함수는 1급 시민입니다. 1급 시민인 함수를 줄여서 **1급 함수**라 부르기도 합니다.



### 화살표 함수 (Arrow Function)

```js
// 매개변수가 하나밖에 없다면, 매개변수 부분의 괄호를 쓰지 않아도 무방합니다.
const negate = x => !x;
```

화살표 함수는 표기법이 간단하기 때문에 익명 함수를 다른 함수의 인수로 넘길 때 주로 사용됩니다.

```js
[1, 2, 3, 4, 5].filter(x => x % 2 === 0);
```



## 제어 구문

### 조건문(Conditional Statement)

#### switch 구문

아래 예제의 `if...else` 구문 예제와 같이 하나의 변수에 대해 많은 경우의 수가 있는 경우, `switch` 구문을 사용하면 코드를 조금 더 보기 좋게 만들 수 있습니다.

`if...else` 구문 예제:

```js
function translateColor(english) {
  if (english === 'red') {
    return '빨강색';
  } else if (english === 'blue') {
    return '파랑색';
  } else if (english === 'purple' || english === 'violet') {
    return '보라색';
  } else {
    return '일치하는 색깔이 없습니다.';
  }
}
```

아래 예제는 바로 위의 코드 예제와 완전히 똑같이 동작합니다.

`switch` 구문 예제:

```js
function translateColor(english) {
  let result;
  switch (english) {
    case 'red':
      result = '빨강색';
      break;
    case 'blue':
      result = '파랑색';
      break;
    case 'purple':
      result = '보라색';
      break;
    case 'violet':
      result = '보라색';
      break;
    default:
      result = '일치하는 색깔이 없습니다.';
  }
  return result;
}
```

`switch` 구문은 `case`, `break`, `default`라는 키워드와 함께 사용됩니다. `switch` 바로 뒤의 괄호의 값이 '코드 실행 여부를 판별할 기준이 되는 값'이고, 이 기준이 되는 값과 `case` 바로 뒤에 오는 값이 **일치**[1](https://helloworldjavascript.net/pages/175-control-statement.html#fn_1)하면 콜론(:) 뒤의 코드 영역이 실행됩니다. 일치하는 값이 없으면 `default` 코드 영역이 대신 실행됩니다.

단, 여기서 주의할 점이 있습니다. `case` 뒤쪽의 코드 영역 마지막에 `break`를 써주지 않으면, 해당 `case`가 실행될 때 바로 뒤의 `case` 코드 영역이 뒤이어 실행되게 됩니다. 예를 들어, 위 코드 예제에서 `case 'blue':` 부분의 `break`를 지우고 코드를 실행해보면 이런 결과가 나옵니다.

```js
translateColor('blue'); // '보라색'
```

위와 같이 함수를 호출하면 `case 'blue':` 뒤쪽의 코드 영역이 실행되는데, 이 안에 `break`를 써주지 않았기 때문에 다음에 나오는 `case 'purple':` 뒤쪽의 코드 영역이 같이 실행되었습니다.

이처럼 `break`를 써 주지 않으면 의도치 않은 동작을 할 수 있으니 주의하세요. 다만 `break`의 이런 성질을 활용해서 코드를 짧게 쓸 수도 있습니다.

```js
function translateColor(english) {
  let result;
  switch (english) {
    case 'red':
      result = '빨강색';
      break;
    case 'blue':
      result = '파랑색';
      break;
    case 'purple':
    case 'violet':
      // 이 코드 영역은 english 변수의 값이 'purple'일 때와 'violet'일 때 모두 실행됩니다.
      result = '보라색';
      break;
    default:
      result = '일치하는 색깔이 없습니다.';
  }
  return result;
}
```



------

### 반복문 (Looping Statement)



#### `do...while` 구문

`do...while` 구문은 `while` 구문과 사용법은 크게 다르지 않으나, **내부 코드를 무조건 한 번은 실행시킨다**는 차이점이 있습니다.

```js
do {
  console.log('do...while!');
} while (false); // 절대 `true`가 될 수 없지만, 루프는 1회 실행됩니다.
```



#### 배열의 순회

ES2015가 나오기 이전까지는 `for` 구문이 배열을 순회하는 데에도 많이 사용되었습니다.

```js
const arr = [1, 2, 3, 4, 5];

for (let i = 0; i < arr.length; i++) {
  console.log(`배열의 ${i + 1} 번째 요소는 ${arr[i]} 입니다.`);
}
```

하지만 근래에는 배열의 `forEach` 메소드나 `for...of` 구문이 더 많이 쓰이는 편입니다.

```js
const arr = [1, 2, 3, 4, 5];

arr.forEach((item, index) => {
  console.log(`배열의 ${index + 1} 번째 요소는 ${item} 입니다.`);
})
const arr = [1, 2, 3, 4, 5];

for (let item of arr) {
  console.log(`현재 요소는 ${item} 입니다.`);
}
```



#### `break`, `continue`

간혹 루프를 도중에 멈추거나, 남은 코드를 건너뛰어버리고 루프의 다음 번 차례로 넘어가야 할 필요가 있습니다. 이 때 사용되는 구문이 `break`와 `continue` 입니다.

```js
alert('퀴즈를 시작합니다.');
while (true) {
  const answer = prompt('빨강의 보색은 무엇일까요?');
  if (answer === '초록') {
    alert('정답입니다! 🎉');
    break; // 루프를 종료하고 다음 코드로 넘어감
  } else {
    alert('틀렸습니다! 다시 시도해보세요.');
  }
}
alert('퀴즈가 끝났습니다.');
for (let i = 1; i < 100; i++) {
  console.log(`현재 숫자는 ${i} 입니다.`);
  if (i % 7 !== 0) {
    continue; // 루프의 나머지 코드를 건너뜀
  }
  console.log(`${i}는 7의 배수입니다.`);
}
```



### 함수를 즉시 종료하기

`continue`와 `break`가 루프의 나머지 코드를 건너뛰는 효과를 갖는 것과 유사하게, `return`과 `throw`는 함수의 나머지 코드를 건너뛰고 함수를 즉시 종료시키는 결과를 낳습니다.

#### `return`

```
return
function translateColor(english) {
  switch (english) {
    case 'red': return '빨강색';
    case 'blue': return '파랑색';
    case 'purple':
    case 'violet': return '보라색';
    default: return '일치하는 색깔이 없습니다.';
  }
}
```

#### `throw`

```js
function translateColor(english) {
  switch (english) {
    case 'red': return '빨강색';
    case 'blue': return '파랑색';
    case 'purple':
    case 'violet': return '보라색';
    default: throw new Error('일치하는 색깔이 없습니다.');
  }
}
```

`throw` 구문은 코드의 실행을 중단시키고 에러를 발생시키는 동작을 합니다.



------

## 객체 (Object)



### 메소드 (Method)

객체의 속성값으로 **함수**를 지정할 수도 있습니다.

```js
const person = {
  greet: function() {
    return 'hello';
  }
};

person.greet(); // 'hello';
```

위와 같이 **어떤 객체의 속성으로 접근해서 사용하는 함수**를 **메소드(method)**라고 부릅니다. 아래와 같이, 객체 리터럴 안에서 특별한 표기법을 사용해 메소드를 정의할 수도 있습니다.

```js
// 위 예제와 완전히 똑같이 동작합니다.
const person = {
  greet() {
    return 'hello';
  }
};

person.greet(); // 'hello';
```



### 생성자 (Constructor)

이제까지는 객체를 생성하기 위해 객체 리터럴 또는 `Object.create` 함수를 사용했습니다. 하지만 이것 말고도 한 가지 방법이 더 있는데, 바로 `new` 키워드를 이용하는 것입니다.

```js
const obj = new Object();
```

위 문장은 `new` 키워드가 붙었다는 것 말고는 **함수 호출** 문법과 비슷하게 생겼는데, 사실...

```js
typeof Object; // 'function'
```

`Object`는 함수입니다! 이렇게 객체를 만들 때 `new` 키워드와 함께 사용하는 함수를 가지고 생성자(constructor)라고 부릅니다.

#### 생성자 정의하기

JavaScript에서는 `Object` 뿐만 아니라, 내장된 많은 생성자들이 있고, 심지어 프로그래머가 직접 생성자를 만들 수도 있습니다. 여기서 `this` 키워드가 한 번 더 등장합니다.

```js
// 생성자 정의
function Person(name) {
  this.name = name;
}

// 생성자를 통한 객체 생성
const person1 = new Person('윤아준');
```

위에서 `function` 구문을 통해 `Person`이라는 생성자를 정의하고, 생성자 안에서는 `this` 키워드를 사용해서 **새로 만들어질 객체의 속성을 지정**해 주었습니다. `new` 키워드를 사용해서 객체를 생성하는 순간에 생성자 안에 있는 코드가 실행되어 객체의 속성이 지정되는 것입니다.

**생성자의 이름**으로는 식별자로 사용할 수 있는 것이면 뭐든지 사용할 수 있지만, 변수와는 다르게 **대문자로 시작**하게끔 짓는 것이 널리 사용되는 관례입니다.



#### 인스턴스 (Instance)

생성자를 통해 생성된 객체를 그 생성자의 **인스턴스(instance)**라고 합니다. 위의 예제에서는 `person1`이 `Person`의 인스턴스입니다. `instanceof` 연산자를 사용하면, 객체가 특정 생성자의 인스턴스가 맞는지를 확인할 수 있습니다.[7](https://helloworldjavascript.net/pages/180-object.html#fn_7)

```js
person1 instanceof Person; // true
```

객체 리터럴을 통해 생성된 객체는 `Object`의 인스턴스입니다.

```js
const obj = {};
obj instanceof Object; // true
```





------

# 2. Today I found out



------

# 3. References



