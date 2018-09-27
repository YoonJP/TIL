# TIL(Today I learned)
# 09/27 (목) 
------

# 1. Today I Learned

## Review

- 표현식(Expression)
  - 값으로 변환될 수 있는 부분

- 변수(Variable)
  - 값을 재사용하기 위해 값에 붙일 이름을 선언하고 그 이름에 값을 대입할 수 있는데 이때 이 이름을 변수라함

## Lecture
#### Tutorial

- 객체
  - 여러 개의 값을 한꺼번에 담아 통(container)처럼 사용할 수 있는 자료구조
  - 객체에는 이름(name)에 값(value)이 연결되어 저장, 이를 이름-값 쌍, 혹은 객체의 속성(property)이라고 함
    ```js
    // 객체의 생성
    const obj = {
      x: 0, // 객체의 속성. 속성 이름: x, 속성 값: 0
      y: 1 // 객체의 속성. 속성 이름: y, 속성 값: 1
    }
    
    // 객체의 속성에 접근하기
    obj.x;
    obj['y'];
    
    // 객체의 속성 변경하기
    obj.x += 1;
    obj['y'] -=1;
    
    // 객체의 속성 삭제하기
    delete obj.x;
    ```
  - 객체의 속성을 통해 사용하는 함수를 메소드(method)라고 함
    - 어떤 객체의 메소드 안에 this가 있으면 그 this는 '메소드가 호출될 때' 해당 객체를 가리키게 된다. 
     ```js
    const obj = {
      x: 0,
      increaseX: function() {
        this.x = this.x + 1;
      }
    };
    
    obj.increaseX();
    console.log(obj.x); // 1
    ```

- 배열
  - 배열은 객체의 일종, 다른 객체와는 다르게 취급
  - 배열을 담는 데이터는 객체의 경우와 달리 요소(element) 혹은 항목(item)이라고 부름
  - 배열 요소 간에는 순서가 존재하며, 이름 대신에 인덱스(index)를 이용해 값에 접근
     ```js
    // 배열의 생성
    const arr = ['one', 'two', 'three'];
    
    // 인덱스를 사용해 배열의 요소(element)에 접근할 수 있습니다.
    // 배열 인덱스(index)는 0부터 시작합니다.
    arr[0]; // === 'one'
    arr[1]; // === 'two'
    
    // 여러 타입의 값이 들어있는 배열
    [1, 2, 3, 'a', 'b', {x: 0, y: 0, name: '원점'}];
    
    // 배열에 요소 추가하기
    arr.push('four');
    
    // 배열의 요소 삭제하기
    arr.splice(3, 1); // 인덱스가 3인 요소부터 시작해서 하나를 삭제합니다.
    ```

#### JavaScript 소개

- JavaScript와 구동환경
  - 구동환경: 웹 브라우저, 웹 서버(Node.js), 게임 엔진, 포토샵 등
  - ES5(2009년)와 ES2015(2015년)부터 그 이후 버전과는 차이가 아주 큼

### JavaScript 기초
#### 값 다루기
- 값(value)과 리터럴(literal)
  - 리터럴은 값의 표기법으로, 프로그래밍 언어마다 값을 표현하는 여러 가지 리터럴을 가지고 있음
  ```js
  1; // 정수 리터럴
  2.5; // 부동 소수점 리터럴
  'hello'; // 문자열 리터럴
  true; // 진리값 리터럴
  ```
- 변수 (Variable)
  - 값에 이름을 붙여서 다시 쓸 수 있게 만드는 기능
  - `let`
    - ES2015부터 사용
    - 변수를 선언할 때 쓰는 키워드
  - `var` 대신 `let`을 쓰는게 best practice
  - `const`
    - `const`는 `let`과 달리 재대입(reassign)이 불가능함
    - `const`는 선언과 대입을 동시에 해줘야됨
    - `const`와 `let`은 동시에 여러 개의 변수를 선언 할 수 있음, 선언한 이름은 다시 선언 할 수 없음

  - `let`과 `const` 중 무엇을 쓸 것인가?
    - best practice는 `let`보다 `const`를 사용하는 것

- 식별자(Idnetifier)
  - 식별자는 변수의 이름으로 쓸 수 있음
    - 숫자, 알파벳, 달러 문자($), 언더스코어(_)가 포함될 수 있음
    - 숫자로 시작되어서는 안 됨
    - 예약어는 식별자가 될 수 없음
    ```js
    const foo; // O
    const _bar123; // O
    const $; // O - jQuery가 이 식별자를 사용합니다.
    const 7seven; // X
    const const; // X - 예약어는 식별자가 될 수 없습니다.
    ```
    - 식별자규칙에 어긋난 식별자는 `''`를 써야 사용 가능
    - 한글 식별자보다 영어 식별자를 쓰는게 best practice
  - Camel Case
    ```js
    // Camel case
    let fastCampus;
    let fooBar;
    let hellowWorldJavaScript
    ```
  - 단어 사이에 언더스코어(_)를 쓰는 Snake case도 있지만 Camel Case의 쓰임이 대부분임

- 타입
  - 값의 종류를 자료형(data type)이라 하고, 줄여서 타입으로 불림
  - 값의 타입을 알아보기 위해 `typeof`연산자를 사용
      ```js
    typeof 1; // 'number'
    typeof 'hello'; // 'string'
    ```

#### number 타입

- number 타입 리터럴
  ```js
  7; // 정수 리터럴
  2.5; // 부동 소수점 리터럴
  0b111; // 2진수 리터럴 (binary literal)
  0o777; // 8진수 리터럴 (octal literal)
  0xf5; // 16진수 리터럴 (hexademical literal)
  ```

- 2진수, 16진수 정수 리터럴은 표기법일 뿐, 내부적으로는 10진수 정수와 같은 형태로 다루어짐
  ```js
  //0x4d는 0b1001101 혹은 77과 완전히 같은 값
  
  0x4d === 77; // true
  0b1001101 === 77; // true
  ```

- 정수인지 실수인지 판별하기
  ```js
  Number.isInteger(1); // true
  Number.isInteger(0.1); // false
  ```

- number 타입에 대해 사용되는 연산자(operator)
  ```js
  // 산술 연산 (arithmetic operators)
  1 + 2; // 더하기
  3 - 4; // 빼기
  5 * 6; // 곱하기
  7 / 8; // 실수 나누기
  14 % 3; // 나머지
  2 ** 3; // 거듭제곱
  
  // 비교 연산 (comparison operators)
  1 < 2; // 작다
  3 > 4; // 크다
  5 <= 5; // 작거나 같다
  6 >= 7; // 크거나 같다
  8 === 8; // 같다 ('=='도 가능하지만 '==='를 쓰는게 best practice)
  8 !== 9; // 같지 않다
  
  // 증가/감소 연산 (incresement/decreasement operators)
  let a = 1; ++a; // 연산결과는 2, a는 2
  let b = 1; b++; // 연산결과는 1, b는 2
  let c = 1; --c; // 연산결과는 0, c는 0
  let d = 1; d--; // 연산결과는 1, d는 0
  
  let a = 1
  a++; // 1 증가시키기 전 값을 표현식의 결과값으로 반환
  ++a; // 1 증가시킨 후의 값을 표현식의 결과값으로 반환
  
  // 할당 연산 (assignment operators)
  // x에 1을 더한 후 다시 x에 할당하기. 결과적으로 x에는 1이 저장됩니다.
  let x = 0;
  x += 1;
  
  // `+=` 연산은 아래 연산과 완전히 같은 동작을 합니다.
  x = x + 1;
  
  // 덧셈 뿐 아니라 다른 모든 산술 연산자에 대해 할당 연산을 할 수 있습니다.
  x -= 1;
  x *= 2;
  x /= 3;
  x %= 4;
  x **= 5;
  ```

- 연산자 우선순위 (Operator Precedence)
  - 묶음(괄호) 연산자가 연산자중에 가장 높은 순위임

- 부동 소수정 (Floating Point) vs 고정 소수점 (Fixed Point)
  - 컴퓨터는 소수를 2진수를 이용해 저장하기 때문에, 10진수 소수를 정확히 다룰 수 없음
    ```js
    // 반올림 오차 (rounding error)
        0.1 + 0.2
    => 0.30000000000000004
    ```
  - 실수 연산을 하는 프로그램을 만들 때에는, 본인이 어떤 유형의 실수 연산을 필요로 하는지 미리 파악한 후, 어느 쪽을 선택할 지 결정

- number 타입의 특이한 값들
  - [IEEE 754](https://ko.wikipedia.org/wiki/IEEE_754) 표준에 정의되어 있는 값들
    ```js
    NaN
    -0
    Infinity
    -Infinity
    ```
  - 문자열을 숫자열로 바꿔주는 함수
    ```js
    parseInt('3')
    => 3
    
    // parseInt함수에 이상한 값을 넣으면 NaN  
    parseInt('asdf')
    => NaN
    ```
  - `Number.isNaN`을 사용한 덧셈 프로그램 예제:
    ```js
    const a = prompt('a: ')
    const b = prompt('b: ')
    const parsedA = parseInt(a)
    const parsedB = parseInt(b)
    
    if (Number.isNaN(parsedA) || Number.isNaN(parsedB)) {
      alert('숫자를 입력해주세요')
    } else {
      alert(parsedA + parsedB)
    }
    
    1 + (2 + 3 + (4 + 5))
    ```
  - NaN
    - 'Not a Number'의 약자, 계산 불가능한 연산의 결과값을 나타내기 위해 사용
    - "NaN은 숫자가 아니기 때문에, 어떤 숫자와도 같지 않다."는 규칙이 있음
    - 즉, NaN은 number 타입인 NaN과 같지 않음
      ```js
      const thisIsNan = NaN;
      
      // 주의! 이렇게 하면 안 됩니다.
      '==='는 숫자에 특화된 연산자이며, 어떤 값이 NaN인지 아닌지를 판별하는 함수에는 '===(등호)'를 사용해서는 안됨
      thisIsNan === NaN; // false
      
      // 이렇게 해야 합니다.
      Number.isNaN(thisIsNan); // true
      Object.is(thisIsNan, NaN); // true
      ```
  - evaluate (평가) - 표현식을 값으로 변환하는 절차
  - -0
    - JavaScript에서 `0`과 `-0`은 별개의 값이지만, 비교 연산을 해보면 결과값이 `true`로 나옴. 즉, 거의 모든 경우에 `0`과 같은 값으로 간주됨
      ```js
      0 === -0; // true
      1 * -0; // -0
      1 + -0; // 1
      ```
    - 예외: `Object.is` 함수는 `0`과 `-0`을 다른 값으로 취급
      ```js
      Object.is(0, -0); // false
      ```
  - Infinity
    - 무한대를 나타내기 위한 값
      ```js
      1 / Infinity; // 0
      1 / -Infinity; // -0
      ```
    - `Number.isFinite` 
      - 어떤 값이 `Infinity`인지 아닌지 판별하는 메소드
      ```js
      const a = prompt('a: ')
      const b = prompt('b: ')
      const parsedA = parseInt(a)
      const parsedB = parseInt(b)
      
      if (Number.isNaN(parsedA) || Number.isNaN(parsedB)) {
        alert('숫자를 입력해주세요')
      } else {
        alert(parsedA + parsedB)
      }
      
      1 + (2 + 3 + (4 + 5))
      ```
- parseInt, parseFloat
  - 문자열을 number 타입으로 바꾸기 위해 `parseInt` 혹은 `parseFloat` 함수를 사용할 수 있음
    ```js
    parseInt('123'); // 123
    parseInt('110', 2); // 6 (문자열을 2진수로 간주한다.)
    parseFloat('12.345'); // 12.345
    parseInt('hello'); // NaN
    ```
- 다른 타입과의 연산
  - number 타입과 다른 타입 간의 연산도 허용하지만, 그 결과가 별로 우아하지는 않음
  ```js
  1 + null; // null
  1 * '1'; // NaN
  1 + '1'; // '11'
  1 - '1'; // 0
  ```
  - number 타입과 다른 타입의 연산은 웬만하면 피하는 것이 좋음
  - 수 연산을 하기 전에 모든 피연산자를 확실히 number 타입으로 만들어주는 것이 좋은 습관

- Math 객체
  - 내장된 `Math` 객체에는 수 연산을 위한 많은 메소드와 상수들이 내장
    ```js
    // 절대값, 올림, 내림, 반올림, 소수점 아래 잘라내기
    Math.abs // 절댓값
    Math.ceil // 올림
    Math.floor // 내림
    Math.round // 반올림
    Math.trunc // 소수점 아래 잘라내기
    
    // 최대값, 최소값
    Math.max
    Math.min
    
    // 랜덤: 0과 1사이의 아무 실수나 반환해 줌
    Math.random
    
    Math.random * 10 // 0과 10사이의 아무 실수나 반환
    
    Math.floor(Math.random() * 10) // 0과 10사이의 아무 정수나 반환
    
    Math.floor(Math.random() * 10) + 10 // 10과 19사이의 아무 정수나 반환
    
    Math.floor(Math.random() * 6) + 1 // 주사위(1~6사이의 아무수나 반환)
    Math.ceil(Math.random() * 6) + 1
    ```
- number 타입의 메소드
  - number 타입은 객체가 아니지만, 마치 객체처럼 메소드를 사용할 수 있음, 래퍼 객체(wrapper object)라는 기능을 제공하기 때문

※ parameter: 매개 변수, 함수에 입력받는 값

※ ``${}``: backtick literal


---

# 2. Today I found out

Math.random의 숫자를 랜덤으로 반환하는 기능이 흥미로웠고 다양하게 쓰일 수 있을 것 같다. 


---

# 3. References

- [연산자 우선순위 표](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/%EC%97%B0%EC%82%B0%EC%9E%90_%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84)
- [컴퓨터로 소수를 표현하는 방식](https://helloworldjavascript.net/pages/130-number.html)

